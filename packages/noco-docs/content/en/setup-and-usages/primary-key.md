---
title: "Primary Key"
description: "Primary Key"
position: 575
category: "Product"
menuTitle: "Primary Key"
---

## What is a Primary Key ?
- A primary key is a special database table column designated to uniquely identify each table record.

## What is the use of Primary Key ?
- As it uniquely identifies an individual record of a table, it is used internally by NocoDB for all operations associated with a record

## Primary Key in NocoDB
- Primary Key that gets defined / used in NocoDB depends on how underlying table was created. Summary is captured below
1. From UI, Create new table / Import from Excel / Import from CSV
    1. An `ID` [datatype: Integer] system field created by default during table creation is used as primary key
    2. Additional system fields `created-at`, `updated-at` are inserted by default & can be omitted optionally; these fields can be deleted after table creation
2. Connect to existing external database
    1. Existing `primary key` field defined for a table is retained as is; NocoDB doesn't insert a new ID field
    2. Additional system fields `created-at`, `updated-at` are not inserted by default
3. Import from Airtable
    1. Airtable record ID is marked as primary key for imported records, and is mapped to field `ncRecordId`  [datatype: varchar]
    2. If a new record is inserted after migration & if ncRecordId field was omitted during record insertion - auto generated string will be inserted by NocoDB
    3. Computed hash value for the entire record is stored in system field `ncRecordHash`
    4. Additional system fields `created-at`, `updated-at` are not inserted by default
4. Create new table using SDK / API
    1. No default primary key field is introduced by NocoDB. It has to be explicitly specified during table creation (using attribute `pk: true`)

## What if Primary Key was missing?
It is possible to have a table without any primary key. 
- External database table can be created without primary key configuration.
- New table can be created using SDK / API without primary key
In such scenario's, new records can be created in NocoDB for this table, but records can't be updated or deleted [as there is now way for NocoDB to uniquely identify these records]

#### Example : Primary Key & optional system fields during new table creation
![Screenshot 2022-06-16 at 12 15 43 PM](https://user-images.githubusercontent.com/86527202/174010350-8610b9c1-a761-4bff-a53d-dc728df47e1b.png)

#### Example : Show System Fields
![Screenshot 2022-06-16 at 12 16 07 PM](https://user-images.githubusercontent.com/86527202/174010379-9e300d42-ad89-4653-afa2-f70fca407ca8.png)

## Can I change the Primary Key to another column within tables ?
- You can't update Primary Key from NocoDB UI. You can reconfigure it at database level directly & trigger `metasync` explicitly
