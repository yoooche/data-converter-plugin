---
name: data-converter
description: Convert data from input .xlsx/.csv files into target template.csv file by column mapping.
disable-model-invocation: true
allowed-tools: Read, Grep, Write, Glob
---

# Data Converter Guide

This skill provides guidance for generating JavaScript code to convert data from input .xlsx/.csv files into target template.csv file by column mapping.

## Step 1:Mapping Columns

1. parse all of columns in ./example/<file-name>.csv.
2. use the letters in the first rows as indexes.
3. use the indexes to find the columns in ./source/$0.
4. complete the mapping table by listing both of two files' column names. ex:

|target_column|source_column|
|---|---|
|用戶名|客戶名|
|余额|用戶餘額|
|上级代理|代理|

## Step 2: Generate JavaScript Data Converting Script
 
1. Analyze the type of source and target files, it would be .csv or .xlsx. Use appropriate libs with different scenarios. When process .xlsx files then import node-xlsx to implement the script.
Use papaparse to process .csv files instead. If the files are both then import both libs.

|.xlsx|.csv|
|---|---|
|`node-xlsx`|`papaparse|

2. Use Node.js Core Modules (fs, stream, path) to process files with massive rows and columns streamly.
3. Use $1 to name the output file.
4. Some of columms have special mapping rules that should be read from ./reference/mapping-rules.md.
5. Generated script should be saved as <root>/src/convert-$0.js.

## Step 3: Execute JavaScript Data Converting Script

1. Run the 1/10 data rows to pre-validate the script.
2. If the script is correct, run the script to convert the entire file.
3. If the script is incorrect, go back to Step 2 and fix the script.

## Step 4: Validate Target CSV File

1. Check the rows number of target CSV file is equal to source file.
2. Check the columns number of target CSV file is equal to template CSV file event if some columns is blank.
3. Check some of columns' value that should be particular format like float, double, date, datetime, etc. 
    - Check Birthday column should be in format of YYYY-MM-DD.
    - Check Other columns related to operation date should be in format of YYYY-MM-DD HH:mm:ss, UTC+8.
    - Check balance column should be in format of float.