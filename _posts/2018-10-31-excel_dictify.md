---
layout: post
title: Tabular Data Auto-Documentation Script
published: true
---

A simple brute-force python 3.6+ program to generate a data-dictionary spreadsheet from any number of tabular files.  
Can undoubtedly be improved in many ways (e.g., PEP8 adherence, error handling, user options, defined functions, etc.)  

```python
from sys import argv # so command line arguments can be accessed
from time import sleep # pauses in the program to ease reading
import pandas as pd # pandas for dataframe management and read/writing excel
import re # regular expressions for string pattern matching

filenames = argv[1:] # python is 0-indexed. index 0 is the name of the script
# this takes us from index 0 to the end of the list of arguments we passed
# when running the script with `python dictify.py x y z`

# User Instructions
print('''
Files to be dictify\'d are passed
as arguments in the command line
e.g. "python dictify.py data1.csv data2.xlsx data3.csv data4.xls"

Make sure the datafile(s) you want to dictify
is/are the in the same folder as this script,
or else specify the absolute path.

Please note: this script will create one excel file with a
data-dictionary sheet for every filename you passed to the command line.

It will not run if you do not have write-privileges to the
directory from which you ran it. Only excel and .csv files are supported.
''')

if len(argv) < 2:
    print('ERROR: Please supply filename arguments and rerun script.')
    quit()
else:
    pass

sleep(6)

print('Files to be dictify\'d:')
for name in filenames:
    print(name)
print('')


# Output preparation
dict_name = input('Please type a name for your data_dictionary > ')
writer = pd.ExcelWriter(dict_name + '.xlsx')

# Meat of the script
for filename in filenames:
    filetype = re.findall(r'(\..*)', filename)[0]

    # Control Flow
    try:
        if filetype == '.csv':
            data = pd.read_csv(filename)
        elif filetype == '.xlsx' or filetype == '.xls':
            data = pd.read_excel(filename)
        else:
            print('Invalid file extension. Please rerun program')
            quit()
    except:
        print('Unknown error occured. Please check files and rerun')
        quit()

    print(f'Dictifying {filename}...')

    # misisng value counts

    na_perc = (data.isnull().sum() / len(data)) * 100

    # Mapping names to their types and missing value counts
    zipper = list(zip(data.columns,
                      data.dtypes.replace({'object':'text',
                                           'int64':'integer',
                                           'float64':'float',
                                           'bool':'boolean'}),
                      na_perc))

    names = ['Field Name', 'Data Type', 'Percent Missing Data']

    # Tidying up
    data_dictionary = pd.DataFrame(zipper, columns = names)
    data_dictionary['Data Set'] = filename
    data_dictionary['Description'] = '' # to be manually entered later
    data_dictionary['Source for description'] = '' # ditto

    # Reordering everything
    data_dictionary = data_dictionary[['Field Name',
                                       'Description',
                                       'Source for description',
                                       'Data Type',
                                       'Percent Missing Data',
                                       'Data Set']]

    # Write to file
    sheetname = re.findall(r'(.*)\..*',filename)[0]
    data_dictionary.to_excel(writer, sheet_name = sheetname, index = False)

writer.save() # saves and closes the dictionary excel file

print('Dictifying complete!')
print(f'Find your file as {dict_name}.xlsx')
sleep(1)
```
