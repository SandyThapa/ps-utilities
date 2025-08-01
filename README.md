# PeopleCode Utilities for Dynamic Record Queries

This repository contains reusable PeopleCode methods designed to help developers count and fetch rows from PeopleSoft records using dynamic filters, ordering, and field selection. Originally built for internal use, the code has been refactored for public deployment to support flexible and maintainable App Classes in PeopleSoft.

## Some might argue dynamic SQL adds memory overhead or complexity and that’s valid. But in PeopleCode, where repetitive logic is common, this approach improves maintainability and clarity. I’m open to feedback and optimisation ideas.


## Features

• Generate dynamic SQL for record queries  
• Count records using flexible filter conditions  
• Fetch rows with ordering and field selection  
• Built for reuse and easy extension in App Classes

## Installation

Copy the provided App Class methods into your PeopleSoft project. Make sure to import the class properly. No external dependencies are required.

This work was shaped during various projects at Griffith University. As a contribution to the broader PeopleSoft community, it’s now available for free use and improvement. We hope it helps and inspires others working in similar areas.

We welcome contributions, enhancements, and feedback. Let’s build a stronger shared codebase together.

## Usage
## FetchRowsFromTable
After importing the class, call the function with your parameters. The filters should be an array of arrays of strings, each containing:

KEY, OPERATOR, VALUE

If you leave out the operator, it defaults to "=". For example:

&filters.Push(CreateArray("Key1", "<", "3"));   /* Key1 < 3 */  
&filters.Push(CreateArray("Key1", "3"));        /* Defaults to Key1 = 3 */  
&filters.Push(CreateArray("Key4", "<=", "4"));  /* Key1 <= 4 */  

&filters.Push(CreateArray("Key3", "in", "GU_GP_ABS_EESS_REQ,CURRENT_ENROL_TERMS"));  /* supports as many values as you want for IN clause as well */'
&filters.Push(CreateArray("Key4", "LIKE", "PS_GP%"));

   

All of the above are valid.

Here’s a full example:

Local string &lStr;  
Local array of string &lTempArry = CreateArrayRept(&lStr, 0);  
&filters = CreateArrayRept(&lTempArry, 0);  

&filters.Push(CreateArray("KEY1", "VALUE1"));  
&filters.Push(CreateArray("KEY2", "VALUE2"));  
&filters.Push(CreateArray("Key3", "in", "GU_GP_ABS_EESS_REQ,CURRENT_ENROL_TERMS"));
&filters.Push(CreateArray("Key4", "LIKE", "PS_GP%"));

Local array of string &selectFields = CreateArray("SELECT_FIELD");  /* Not used yet, but planned for future improvements */  
Local array of string &orderBy = CreateArray("EFFDT DESC");  
Local string &recName = "TABLE_NAME";  

&model.FetchRowsFromTable(&filters, &orderBy, &selectFields, &recName);


## GetCount
&countFilters = CreateArray(
    CreateArray("KEY1", "=", "VALUE1"),
    CreateArray("KEY2", ">", "VALUE2")
);

Local Record &tableName = CreateRecord(Record.RECORD_NAME);

Local string &countfield = "*";  

&notifCount = &model.GetCount(&tableName, &countFilters, countfield);

