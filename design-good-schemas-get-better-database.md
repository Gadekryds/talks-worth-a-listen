# Use natural keys

A Country table doesn't require a Sequential key.
The number 1, 2, 3 requires links to the Country table to become useful.
You cannot use the ID for anything else.

Using the Alpha-2 or Alpha-3 codes would instead be more useful in most cases.
You can avoid linking the Country table, if all you need is to know what country it is.
e.g. If the foreign key of your link is "DK" you wouldn't need to look up the number 12.

# Store atomic values

Don't be "smart" with your values in tables.
FirstName and LastName should be in different columns.
You cannot sort from database side, by having these in a FullName column.

# Use concise data types

Don't use varchar/text for datetime.

# No ORM

*ORMs can quickly trip you up.
It might be good in a small project, but in the long run? 

ORMs are good for: 
* Connection Management
* Type Encoding / Decoding (CODEC)

But you can quickly run into more trouble than help using an ORM for
* Command Marshalling
* Abstraction
* Change Detection

# Scripted, source-controlled DDL

Using Migration Scripts
* Liquibase
* Flyway
