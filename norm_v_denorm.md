# Tradeoffs between Normalization and Denormalization 

## Normalization

Normalization is a way of defining database schema that optimizes fast
writes as well as high integrity by ensuring no redundant data are created
across the table.

It requires more index hits but usually results in smaller tables and higher
information density. Need more indexes to manage data

Normalization has faster writes because the database doesn't need to update
/inserts the data in multiple locations but only a single one.

Why does normalization make indexing less efficient/ more complicated?

Since there are no redundany data, the storage required for new records to 
any table are relatively small

Writes are also guaranteed to leave database in a consistent state due to
referential integrity guarantees from foreign key

*Referential Integrity:* refers to the accuracy and consistency of data within a 
relationship. All references are valid.

## Normal Form 1-3 for Normalization
    
    -1NF: if the domain of each attribute contains only atomic values, and the
    value of each attribute contains only a single value from that domain
        -Elminate repeating groups in individual tables
        -Create a separate table for each set of related data
        -Identify each set of related data with a primary key
    -2NF:
        -Be in 1NF
        -All the non-key columns are dependent on the table's primary key
        ("Does this column serve t odescribe what the priamry key identifies?")
    -3NF:
        -Be in 2NF
        -Only foreign key columns sohuld be used to reference another table's
        information, no other columns from the parent table should exist in the
        referenced table. 

## Denormalization

Denormalization is the concept of having redundancies in a schema to optimize
reads that would otherwise be expensive in a normalized schema.

In a normalized table, pieces of data are distributed among many tables and can
require complex joins to get the full picture of a query.

It could make sense to store some values redundantly in your system to speed 
up query performances

---Denomralized values makes inserts and updates trickier --- 
    -Need to ensure that denormalized values are properly maintained 
    due to integrity of these values not being enforced by schema
    -Denomralize should be documented and tested and communicated across
    teams

## Normalization vs Denormalization

Why are updating easier in a normalize table vs denormalize?
Normalizing data make updates a breeze

Normalize database also optimizes for some kinds of reads although reads
where data are across multiple tables become more challenging due to complex
joins. If tables are big, queries can become slow compared to denormalized.

For denormalizing, query for unique things might be harder because the entire table
needs to read compared to a normalize table where the table  .... Updates are
also trickier because we need to ensure integrity of our data logically. Updates
will need to be made in multiple places.
