# Python codes for data transformation

## Section 1

*Read Excel files*
```
import pandas as pd
df = pd.read_excel( 
"J:\\1_Ren21\\Final..Final\\organization.import\\organizations_ren21___28_12_2023_10-58_am.xlsx", sheet_name='Sheet1')
```

*Read CSV file by specifying columns data types*
```
#Csv file path
excel_file_path = "J:\\1_Ren21\\contactImport1\\Contacts.csv"

dataType = {
 'RecordId': str,
 'Salutation': str,
 'FirstName': str,
 'LastName': str,
 'Organization': str,
 'Role': str,
 'Background': str
}

dataFrame = pd.read_csv(excel_file_path, sep=',', encoding='utf-8', 
dtype=dataType, skipinitialspace=True)

```

*Read text file*
```
f1 = open(file_path, encoding="utf8")
g1 = f1.read()
```

*Get size of a dataframe*
| Organization | City | Country | Email |
|---|---|---|---|
|ABC CONSULTING | Yaounde| Cameroon| yvontjs@gmail.com| 
|ARIK AIR | Lagos| Nigeria| bertino@yahoo.com| 
|CONGELCAM  |  Doula | Cameroon| yvontjs@gmail.com| 
```
import pandas as pd
df = pd.read_excel("J:\\1_Ren21\\Final..Final\\organization.import\\test_org.xlsx", sheet_name='Sheet1')
print(len(df))
```
Output: 3

*Split a dataframe depending on a condition*
Example 1: Condition on string length
- Define a function
- Apply the function to the dataframes

```
def check_len(text):
 if len(str(text)) >= 300:
 return True
 else:
 return False
dataFrame1 = df[df["OrganizationName"].apply(check_len)]
dataFrame2 = df[~df["OrganizationName"].apply(check_len)]
```
```
list_of_strings = ['html', 'husband', 'Click', 'Looking', 'doing', 'Awesome', 'Feel', 'Follow']
def remove_records_with_urls(url_string):
 if any(substring in str(url_string) for substring in list_of_strings):
 return False
 else:
 return True
# Applying function to dataframe
dataFrameFreeFromDoubleQuoteHyperLink = dataFrame[dataFrame["OrganizationName"].apply(remove_records_with_urls)]
dataFrameWithDoubleQuoteHyperLink = dataFrame[~dataFrame["OrganizationName"].apply(remove_records_with_urls)]
```

*Exporting dataframes*
Export to Excel
```
dataFrame1.to_excel("J:\\1_Ren21\\Final..Final\\organization.import\\dataFrame1.xlsx", index=False)
dataFrame2.to_excel("J:\\1_Ren21\\Final..Final\\organization.import\\dataFrame2.xlsx", index=False)
```
Export to csv
```
DataFrameExploded.to_csv("J:\\1_Ren21\\contactImport1\\1-1-dataFrameExploded.csv", index=True)
```

*Convert dataframe column into list*
```
organization_col_list = df["Organization"].values.tolist()
```

*Convert dataframe in str*
```
dataFrame = dataFrame.astype(str)
```

*Check if ascii character*

```
def troubleshoot_ascii_characters(string_text):
 if str(string_text).isascii():
 return True
 else:
 return False
```

*Check if a string is a mix of letter(s) and number(s)*
```
def containsLetterAndNumber(input):
 if input.isalnum() and not input.isalpha() and not input.isdigit():
 return True
 else:
 return False
```

*Finding the number of upper- and lower-case letter in a string*
```
def case_counter(text_string):
 checkup = 0
 for item in str(text_string).split():
 lower = 0
 upper = 0
 for char in item:
 if char.islower():
 lower = lower + 1
 elif char.isupper():
 upper = upper + 1
 if upper >= 2 and lower >= 1:
 checkup = 1
 break
 else:
 checkup = 0
 if checkup == 1:
 return True
 else:
 return False
```

*From list of strings to list of sets*
```
def from_string_list_to_string_set(my_list):
 final_list = []
 for item_list in my_list:
 item_list = str(item_list).replace(' ', '')
 final_list.append(set(item_list.split('|')))
 return final_list
```

*Remove all space from string using regex*
```
import re
def remove_all_space(string_text):
 return re.sub(r'\s', '', str(string_text))
```

*Email validator with regex*
```
import re
regex = r'^(?:\S+@\S+\.\S+)(?:\s*,\s*\S+@\S+\.\S+)*\b'
def email_validator(email):
 if re.fullmatch(regex, str(email)):
 return True
 else:
 return False
```

*Count duplicate records in dataframe column*
```
import pandas as pd
df = 
pd.read_excel("J:\\1_Ren21\\Final..Final\\organization.import\\test_org.xls
x", sheet_name='Sheet1')
df_count = df.apply(lambda x: x.duplicated()).sum()
print(" Detected number of duplicate : ", df_count['Email'])
```

*Get dataframe of duplicate records*
```
ids = df["lowerEmail"]
df[ids.isin(ids[ids.duplicated()])].sort_values("lowerEmail")
df_4_duplicates = pd.concat(g for _, g in df.groupby("lowerEmail") if 
len(g) > 1)
df_4_duplicates.to_excel("J:\\1_Ren21\\Final..Final\\contact.import\\df_4_d
uplicates.xlsx", index=False)
```

*Merging duplicated records*
```
import pandas as pd
df = pd.read_excel(
 "J:\\1_Ren21\\Final..Final\\organization.import\\test_org.xlsx",
 sheet_name='Sheet1')
dataFrameMerged = df.groupby(['Email']).agg(
 {
 'Email': ' ; '.join,
 'City': ' ; '.join,
 'Country': ' ; '.join,
 'Organization': ' ; '.join
 }).reset_index(drop=True)
print(dataFrameMerged)
```

*Removing specific string from text*
```
def get_rid_of_nan(string_text):
 if (string_text is not None) and ('nan' in string_text.split()):
 new_list = str(string_text).replace(' ', '').split('|')
 for item in new_list:
 if item == 'nan':
 index = new_list.index(item)
 new_list[index] = ''
 if (' '.join(new_list)).strip() is not None:
 return (' '.join(new_list)).strip()
 else:
 return ''
 else:
 return string_text
```

*Converting txt file into list*
```
def open_file_and_convert_in_list(file_path):
 f1 = open(file_path, encoding="utf8")
 g1 = f1.read()
 h1 = g1.split(',') # turning the file into list
 h1[-1] = h1[-1].strip()
 h1 = [x.strip(" ") for x in h1]
 return h1
```

*Replacing fake record in a dataframe*
Let us assume we have such a column in a dataframe
OtherAddressCountry
JMKBapbWfUtQHNLk| WsSOlAMLU
7FEAUYu4aGAs0
Kansas City
R3GaqWmH
nCMdrxsVfYZHXRFv| wuvhYGgmPR

We want to define in a text file different values we want to get rid of.
- First step is to add values we want to remove in a file (text or Excel or csv)
JMKBapbWfUtQHNLk| WsSOlAMLU, 7FEAUYu4aGAs0, R3GaqWmH, nCMdrxsVfYZHXRFv| wuvhYGgmPR
- Next, we convert text file into list
- Next, we should convert list into set
- Then we add another function to do the final job

```
import pandas as pd
path = "J:\\1_Ren21\\Final..Final\\contact.import\\testing_file.txt"
path2 = "J:\\1_Ren21\\Final..Final\\contact.import\\test.xlsx"
df = pd.read_excel(path2)
def open_file_and_convert_in_list(file_path):
 f1 = open(file_path, encoding="utf8")
 g1 = f1.read()
 h1 = g1.split(',') # turning the file into list
 h1[-1] = h1[-1].strip()
 h1 = [x.strip(" ") for x in h1]
 return h1
def convert_list_into_set(my_list):
 final_list = []
 for item_list in my_list:
 item_list = str(item_list).replace(' ', '')
 final_list.append(set(item_list.split('|')))
 return final_list
def replace_rows_with_fake_records_by_empty(url_string):
 new_url_string = str(url_string).replace(' ', '')
 set_url_string = set(new_url_string.split('|'))
 if set_url_string in 
convert_list_into_set(open_file_and_convert_in_list(path)):
 return ''
 else:
 return str(url_string)
df['OtherAddressCountry'] = 
df['OtherAddressCountry'].apply(replace_rows_with_fake_records_by_empty)
print(df)
```

*Remove repetitive substring in a global global string*
```
def check_same_value_in_list(string_text):
 check_list = str(string_text).split('|')
 new_list = str(string_text).replace(' ', '').split('|')
 if new_list.count(new_list[0]) == len(new_list):
 return check_list[0]
 else:
 return string_text
```

*Lower case a string text*
```
def lower_case(string_email):
 if str(string_email) != '':
 return str(string_email).lower()
```


*Partially print dataframe column*
```
print(df['lowerEmail'].head(5))
```

*Take one occurrence of a text with repeated values*
```
def take_one_value(text):
 list_items = str(text).split()
 return list_items[0].strip()
```

*Reformat a string having multiple separators*
```
def reformat_tag(tag):
 new_tag = str(tag).replace('|', '$')
 new_tag1 = new_tag.replace(' ', '')
 list_tag = new_tag1.split('$')
 set_tag = set(list_tag)
 # convert Set to String
 final_tag = '$'.join(set_tag)
 final_tag1 = final_tag.replace('$$', '$')
 return final_tag1
```

*Counting number of occurrence of a string under a dataframe column*
```
utilities_count1 = df['ContactTag_Merged_Id'].str.contains('None').sum()
if utilities_count1 > 0:
 print("There are {m} none".format(m=utilities_count1), 'under 
ContactTag_Merged_Id')
```

*Convert dataframe column into str*
```
df['ContactTag1'] = df['ContactTag1'].astype(str)
df['ContactTag2'] = df['ContactTag2'].astype(str)
df['ContactTag3'] = df['ContactTag3'].astype(str)
df['ContactTag4'] = df['ContactTag4'].astype(str)
df['ContactTag5'] = df['ContactTag5'].astype(str)
```

*Merge dataframe column using a separator*
```
df['ContactTag_Merged'] = df[
 ['ContactTag1', 'ContactTag2', 'ContactTag3', 'ContactTag4', 'ContactTag5', 'ContactTag6', 'ContactTag7', 'ContactTag8', 'ContactTag9']].apply(lambda x:'*'.join(x.dropna()), axis=1)
```

*Reformat a string with multiple separators*
```
def split_tags(tag_string):
 reformated_string0 = str(tag_string).replace('*nan', 
'').replace('nan*', '')
 reformated_string = str(reformated_string0).replace('|', '*')
 reformated_string2 = reformated_string.split('*')
 stripped = list(map(str.strip, reformated_string2))
 final_list = list(dict.fromkeys(stripped))
 final_string = '$'.join(str(e) for e in final_list)
 return final_string
```

*Dynamically split dataframe column using a string separator*
Description: We have a dataframe with a column having values like 
“2773$323233$4433$43344$43349$88879”
We want to get each value from the string to be in a single column
```
# split column into multiple columns by delimiter
df2 = df['ContactTag_Merged'].str.split('$', expand=True)
df2.columns = ['Tag{}'.format(x+1) for x in df2.columns]
df = df.join(df2)
```

*Build a dictionary to replace string values by other values*
```
tag_list = ['Web Contact', 'IREC_Delhi', 'GDPR_InfoGen','Location_Nicaragua', 'Topic_CCUS$GSR_Topic_ETechnologies', 'GSR_Topic_Cost', ...'']
```
We create a list of numeric values
```
tag_values = list(range(3000, 3465))
```
```
str_list = list(filter(None, tag_list))
tag_dict = dict(zip(sorted(str_list), tag_values))
```
```
def set_tag_id_in_contact(item_name):
 long_string = str(item_name).split('$')
 for i in range(len(long_string)):
 if str(long_string[i]).strip() != '':
 long_string[i] = tag_dict.get(str(long_string[i]).strip())
 reformed_string = '$'.join(map(str, long_string))
 return reformed_string
```
```
df['ContactTag_Merged_Id'] = df['ContactTag_Merged'].apply(set_tag_id_in_contact)
```

*Filter dataframe on basis of column value*
```
df1 = df[df.ContactTag_Merged_Id == "topic_renewable_energy"]
print(len(df1))
```

*Replace empty string by np.nan*
```
Replace any empty string in the OrganizationName column of the dataframe with NAN
dataFrame['OrganizationName'].replace(' ', np.nan, inplace=True)
```

*Drop dataframe rows having NA values*
```
dataFrame.dropna(subset=['OrganizationName'], inplace=True)
```

*Check if a string is digit*
```
def check_if_string_is_digit(string_number):
 if str(string_number).isdigit():
 return True
 else:
 return False
```

*Get string with no vowels*
```
def remove_orgName_with_no_vowels(check_O_N):
 if bool(re.match('^[^aeyiuoAEYIUO]+$', str(check_O_N))):
 return False
 else:
 return True
```

*Dropping entirely duplicate records*
```
dataFrameFreFromCompletelyDuplicate = 
dataFrameWithoutSpecialCharacter.drop_duplicates()
```

*Replace np.nan by empty string*
```
dataFrameFreFromCompletelyDuplicate.replace({np.nan: ''}, inplace=True)
```

*Converting columns from object dtype to string*
```
dataFrameFreFromCompletelyDuplicate['BillingAddressCity'] = dataFrameFreFromCompletelyDuplicate['BillingAddressCity'].astype("string") dataFrameFreFromCompletelyDuplicate['BillingAddressCountry'] = dataFrameFreFromCompletelyDuplicate['BillingAddressCountry'].astype("string")
```

*Convert string into list using empty string as separator*
```
def convert_string_to_list(string):
 li = list(string.split(" "))
 return li
```

*Convert string into uppercase*
```
def upercase_fct(text_string):
 return str(text_string).upper()
```

*Apply() method*
It helps to apply a method on each dataframe row.
The function will process on each email address
```
def remove_space(string_text):
 return re.sub(r'\s', '', str(string_text))
dataFrame["EmailAddress"] = dataFrame["EmailAddress"].apply(remove_space)
```

*Split()*
```
def transform_string_email_in_ist(string_email):
 return str(string_email).split(';')
```

*Explode()*
Let us assume we have several emails on a column and we want to get each email on a single row.
First step will be to transform emails into list of emails, then apply the explode() method.
| Organization | City | Country | Email |
|---|---|---|---|
|ABC CONSULTING | Yaounde| Cameroon| yvontjs@gmail.com; zaic@hotmail.com| 
|ARIK AIR | Lagos| Nigeria| bertino@yahoo.com| 
|CONGELCAM  |  Doula | Cameroon| yvontjs@gmail.com;  mich@outlook.fr; ben@gmail.com| 

```
import pandas as pd
df = pd.read_excel("J:\\1_Ren21\\Final..Final\\organization.import\\test_org.xlsx", sheet_name='Sheet1')
def transform_string_email_in_list(string_email):
 return str(string_email).split(';')
df['Email'] = df['Email'].apply(transform_string_email_in_list)
DataFrameExploded = df.explode('Email')
DataFrameExploded.to_excel("J:\\1_Ren21\\Final..Final\\organization.import\\dataFrameExploded.xlsx", index=True)
```
| Organization | City | Country | Email |
|---|---|---|---|
|ABC CONSULTING | Yaounde| Cameroon| yvontjs@gmail.com| 
|ABC CONSULTING | Yaounde| Cameroon| zaic@hotmail.com| 
|ARIK AIR | Lagos| Nigeria| bertino@yahoo.com| 
|CONGELCAM  |  Doula | Cameroon| yvontjs@gmail.com|
|CONGELCAM  |  Doula | Cameroon| mich@outlook.fr|
|CONGELCAM  |  Doula | Cameroon| ben@gmail.com|


*Get list of records that are processed and not imported*
This is the scenario: A console App processed data in the CRM and some import failed. We need to know records that have not been imported in the CRM.
We have to build a file with two columns of identifier, maybe the Email. So, we will have Email1 and Email2 that are emails in initial file and those imported in the CRM (to get these one we will export it from CRM).
Then we will process that file with the below python code:
```
import pandas as pd
df_ext = pd.read_excel(J:\\1_Ren21\\Final..Final\\contact.import\\contacts_ren21___04_01_2024_14.30.pm.xlsx", sheet_name='Sheet1')
df_check = pd.read_excel("J:\\1_Ren21\\Final..Final\\contact.import\\check.xlsx", sheet_name='Sheet1')
df_check_ser1 = df_check['EmailAddress1']
df_check_set1 = set(df_check_ser1)
df_check_ser2 = df_check['EmailAddress2']
df_check_set2 = set(df_check_ser2)
set_diff = df_check_set1.difference(df_check_set2)
list_diff = list(set_diff)
def get_fake(emailAdress):
 if str(emailAdress) in list_diff:
 return True
 else:
 return False
dataFrameDiff = df_ext[df_ext["EmailAddress"].apply(get_fake)]
dataFrameDiff.to_excel("J:\\1_Ren21\\Final..Final\\contact.import\\diff10.xlsx", index=False)
print(len(list_diff))
print(len(df_check_set2))
```


## Section 2

```
--- Python code to read a csv file using pandas

######
# 1) Import pandas library
import pandas as pd

# 2) Define csv file path
	# We should use double anti-slash as well in the file path
csv_file_path = "J:\\1_Ren21\\organizationImport\\Organizations.csv"

# 3) Create a DataFrame to read the csv file
df = pd.read_csv(csv_file_path)

--- Python code to get dataFrame number of rows
# 'df' is the panda dataFrame we want to get the number of rows
len(df)

--- Python code to convert a dataFrame column in String data type
# 'df' is the dataFrame and 'OrganizationName' is the dataFrame column name
df['OrganizationName'] = df['OrganizationName'].astype("string")

--- Python code to replace empty string under a dataFrame column with NAN
# 'df' is the dataFrame and 'OrganizationName' is the dataFrame column name
df['OrganizationName'].replace(' ', np.nan, inplace=True)

--- Python code to remove all rows having NAN value under a dataFrame column
# 'df' is the dataFrame and 'OrganizationName' is the dataFrame column name
df.dropna(subset=['OrganizationName'], inplace=True)

--- Python code to convert columns from object dtype to string
# 'df' is the dataFrame 
df.replace({np.nan: ''}, inplace=True)

--- Python code to merge partially duplicated values
# 'df' is the dataFrame, # 'df_merged' is the merged dataFrame; 'OrganizationName', 'Fax', 'Phone' and 'Website' are the different columns to be merged; '$' sign is used as separator
df_merged = df.groupby(['OrganizationName']).agg(
	{
		'Fax': ' $ '.join, 
		'Phone': ' $ '.join, 
		'Website': ' $ '.join
	}).reset_index()
	
--- Python code to export a dataFrame to csv
# 'df' is the dataFrame
df.to_csv("J:\\1_Ren21\\organizationImport\\file_name.csv", index=True)

--- Python code to read Excel file using pandas

######
# 1) Import pandas library
import pandas as pd

# 2) Define Excel file path
	# We should use double anti-slash as well in the file path
excel_file_path = "J:\\1_Ren21\\organizationImport\\Organizations.xlsx"

# 3) Create a DataFrame to read the csv file
# 'df' is the dataFrame
df = pd.read_csv(excel_file_path)

--- Python code to convert a dataFrame column into list
# 'df' is the dataFrame
# 'OrganizationName' is the column name
# 'listFromDataFrameCol' is the list name built from the dataFrame column
listFromDataFrameCol = df['OrganizationName'].tolist() 

--- Python function to convert items (having a separator) from a list into set 
# The purpose is to build a subset of items containing unique values

def convert_list_into_set(my_list):
    final_list = []
    for item_list in my_list:
        item_list = str(item_list).replace(' ', '')
        final_list.append(set(item_list.split('|')))
    return final_list
	
print(convert_list_into_set(['a | b | a', 'e|d', 'c|  k| c'])) ==> [{'b', 'a'}, {'e', 'd'}, {'k', 'c'}]

--- Python code to replace items that exist in a set by empty
# The scenario is we have values that can change positions and we want to remove them no matter their positions
# For instance: we can have at first program run "'a | b | a', 'e|d', 'c|  k| c'", second execution we rather have "'b | a | a', 'e|d', 'k|  c| c'" ...
# The reference set containing items to be removed is 'set_bad_values'

def replace_rows_with_fake_records_by_empty(url_string):
    new_url_string = str(url_string).replace(' ', '')
    set_url_string = set(new_url_string.split('|'))
    if set_url_string in set_bad_values:
        return ''
    else:
        return str(url_string)

--- Python code to remove values that are part of a predefined list
# 'list_of_strings' is our list containing values of fake items

def remove_records_fake_records_special_characters(url_string):
    if any(substring in str(url_string) for substring in list_of_strings):
        return ''
    else:
        return str(url_string)

--- Python code - apply() method on a dataFrame column
# 'df' is our dataFrame and 'OrganizationName' is the dataFrame column
# 'remove_records_fake_records_special_characters' is our function
df["OrganizationName"] = df["OrganizationName"].apply(remove_records_fake_records_special_characters)

--- Python code to remove all rows under a dataFrame having empty values under a specific dataFrame column
# 'df' is our dataFrame and 'OrganizationName' is the dataFrame column
df = df.loc[df['OrganizationName'] != '', :]

--- Python code to decode in "utf-8" a string encoded in 'ISO-8859-1'
def encode_file(column_name):
    _encoding = 'ISO-8859-1'
    return str(column_name).encode(_encoding, 'ignore').decode("utf-8", 'ignore')

--- Python code to remove all empty string that surrounds a string
def remove_space(string_text):
    return str(string_text).strip()
	
--- Python code to remove comma from a string
def remove_coma(string_text):
    return str(string_text).replace(',', ' ')

--- Python code to split that dataFrame in two on a criteria basis
# Let us assume we have a dataFrame with some rows containing double quote and we want to split the dataFrame in two: rows containing double quote and not
def check_if_contains_double_quote(string_text):
    if '"' in str(string_text):
        return True
    else:
        return False
dataFrameOrgWithoutDoubleQuote = dataFrameMerged[dataFrameMerged["OrganizationName"].apply(check_if_contains_double_quote)]		
dataFrameOrgWithDoubleQuote = dataFrameMerged[~dataFrameMerged["OrganizationName"].apply(check_if_contains_double_quote)]

--- Python code to remove rows having no vowels contained in dataFrame column value using regular expression
def remove_orgName_with_no_vowels(check_O_N):
    if bool(re.match('^[^aeyiuoAEYIUO]+$', str(check_O_N))):
        return False
    else:
        return True

--- Python code to count duplicate records 
# Reference column is 'OrganizationName', 'dataFrame_' is the dataFrame
print("Detecting duplicate records")
df = dataFrame_.apply(lambda x: x.duplicated()).sum()
print("Detected number of duplicate in OrganizationName Column: ", df['OrganizationName'])

--- Python code to drop duplicated values under a dataFrame
dataFrame = dataFrameWithoutSpecialCharacter.drop_duplicates()

--- Python code: Split a string and check if each substring is part od English dictionary
def Convert(string):
    li = list(string.split(" "))
    return li


def get_rid_of_fake_org_names(check_O_N):
    d = en.Dict("en_US")
    new_org_name = Convert(str(check_O_N))
    temp = 0
    for item in new_org_name:
        if item != "" and d.check(item):
            temp = 1
            break
    if temp == 1:
        return True
    else:
        return False

--- Python code to check if a character s ascii
def troubleshoot_ascii_characters(string_text):
    if str(string_text).isascii():
        return True
    else:
        return False

--- Python code to remove anti slash from string
def remove_anti_slash_xa0_from_string(text_value):
    return str(text_value).replace(r"\xa0", " ")

--- Python code to check if a string contains only letters and number
def containsLetterAndNumber(input):
    if input.isalnum() and not input.isalpha() and not input.isdigit():
        return True
    else:
        return False

--- Python code to find the number of upper and lower case letter in a string and check if string is part of english dictionary
def case_counter(text_string):
    d = en.Dict("en_US")
    checkup = 0
    double_check = 0
    for item in str(text_string).split():
        lower = 0
        upper = 0
        for char in item:
            if char.islower():
                lower = lower + 1
            elif char.isupper():
                upper = upper + 1
            if d.check(item):
                double_check = 1
        if upper >= 2 and lower >= 1:
            checkup = 1
            break
        else:
            checkup = 0

    if checkup == 1 and double_check == 0:
        return True
    else:
        return False

def case_counter_1(text_string):
    checkup = 0
    for item in str(text_string).split():
        lower = 0
        upper = 0
        for char in item:
            if char.islower():
                lower = lower + 1
            elif char.isupper():
                upper = upper + 1
        if upper >= 2 and lower >= 1:
            checkup = 1
            break
        else:
            checkup = 0

    if checkup == 1:
        return True
    else:
        return False
		
--- Python code to specify the dtype of excel columns we are want to read with pandas
csv_file_path = "J:\\1_Ren21\\contactImport1\\Contacts.csv"
dataType = {
    'RecordId': str,
    'Salutation': str,
    'FirstName': str,
    'LastName': str,
    'Organization': str,
    'Role': str,
	}
dataFrame = pd.read_csv(csv_file_path, sep=',', encoding='utf-8', dtype=dataType, skipinitialspace=True)

--- Python code to explode data from a column
# We have a dataFrame with Email column sometimes containing multiple emails separated by a semi column
# We want each email to be on a single rows
# First step is to transform semi column separated emails into a list
def tansformStringEmailInList(string_email):
    return str(string_email).split(';')
dataFrame["EmailAddress"] = dataFrame["EmailAddress"].apply(tansformStringEmailInList)
# Next we can apply the explode() function
DataFrameExploded = dataFrame.explode('EmailAddress')

--- Python code to remove repetitive items in all rows
# We first define the list of dataFrame columns
list_of_columns = ['RecordId', 'Salutation', 'FirstName', 'LastName', 'Organization', 'Role']
for col in list_of_columns:
    dataFrameMerged[col] = dataFrameMerged[col].str.split(", ").map(set).str.join("| ")

```
