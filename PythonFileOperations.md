# 
# 

import os, sys, string
import csv, time

# read csv file with dog structute
with open('DOG_LinkTable_20160901T1245.csv', 'rb') as f:
    reader = csv.reader(f)
    dog_field_list = list(reader)

#open file for SQL insert into commands
timestr = time.strftime("%Y%m%dT%H%M%S")
fname = 'dog_sql_%s.sql' % timestr
ofile = open(fname, 'w')

#print dog_field_list

proto_sql = '''INSERT INTO _clean_full_table(
 "S_ID", "Year", "Breed", "First_Name", "Last_Name", "Full_Address", "AddressN", "Street",
"Apt_Unit", "Mailing_City", "State", "Zip", "FileName", "FileDate")
SELECT "ID", '', breed, "First name", "Last name", '', "street number", "street name", 
'', "Mailing_City", 'MA', '', 'Acton clean dog data', '06/29/2016'
FROM "Acton clean dog data";'''

dest_table = dog_field_list[0]
# print str(dest_table).strip('[]')
sql_insert_into = '''INSERT INTO %s (%s) SELECT %s FROM %s;'''

version = '-- verion 0.1 from %s\n' % timestr
ofile.write('-- SQL INSERT INTO Structure for DOG Database\n')
ofile.write(version)
ofile.write('-- Myriad Development 2016\n')
ofile.write('-- cretor: Todor Lubenov\n')
ofile.write('-- e-mail: tlubenov@myriad-development.com\n\n\n\n')

for el in dog_field_list:
    into_table = '_clean_full_table'
    into_struct = str(dest_table).strip('[]')
    into_struct = into_struct.replace("'", '"')
    from_fields = str(el).strip('[]')
    from_table = str(el[-3:-2]).strip('[""]')
    from_table = from_table.replace("'", '"')
    sql_insert_into = '''INSERT INTO %s (%s) SELECT %s FROM %s;''' % (into_table, into_struct, from_fields, from_table)
    sql_header = '-- INSERT INTO %s ... FROM %s ...' % (into_table, from_table)
    ofile.write(sql_header)
    ofile.write('\n')
    ofile.write(sql_insert_into)
    ofile.write('\n\n\n\n')
    #print sql_insert_into

ofile.close()

