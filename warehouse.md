Data Warehouses:
e

- relational database that is designed for query and analysis rather than for transaction processing
  Characteristics of Data Warehouses:
- Subject Oriented
  - Data warehouse that concentrates on sales
  - Use to anwser questions like “Who was our best customer for this item last year?”
- Integrated
  - Data warehouses must put data from disparate sources into a consistent format
  - Resolve problems such as naming conflicts and inconsistencies among units of measure
- Nonvolatile
  - once entered into the data warehouse, data should not change.
- Time Variant - Data warehouse focus on change over time.
  Data warehoues generally has many indexes, some joins, denormalized

They are updated on a regular basis by the ETL process (nighly or weekly) using bulk data modification techniques.

Data Warehouse Architecture:

Data source (operational system, flat fiels)
user (analyiss, reporting, mining)
warehouse(metadata, summary data, raw data)

There is a staging area where data is cleaned and processed before putting it into the warehouse

Data marts:

- add data marts, which are systems designed for a particular lien of business, where purchasing, sales, and inventories are separated.

Extract informatio nfrom a data warehouse:

Data Mining:

- Datamining uses large quantities of data to create models. These models can provide insights that are revealing, significant, and valuable.

Logival Vs Physical Desing in Data Warehouses:
Define

- Specific data content
- relationships within and between groups of data
- System environemnt supporting your data warehouse
- The data transformations required
- The frequency with which dat is refereshed
  Logical Design is more conceptutal and abstract tha nthe physical design:
- Look at logical relationships among the objects
  Phsyical Design:
- Look at the most effective way of storing and retrieving the objects as well as handling them from a transportation and backup/recoerby perspective
  End users typically want to perform analysis and look at aggregated data, rather than at individual transactions

Logical Design:
Focus on the information requirements and save the implementation details for later
Creating A Logical Design:

- One way is entity-relationship modeling
  Schema:
- Start Schemas - Optimizes performance by keepign queries simple and providing fast response time. All the information about each level is stored in one row
  Data Warehousing Objects:
- Fact tables:

  - Large tables in your data warehouse schema that store business measurements. Typically contain facts and foreign keys to the dimension tables.Represent data, usually numeric and additive, that can be analyed and examined
  - Usually contain two tpyes of columns, numeric facts (measurement) and foreign keys to dimension tables
  - Contain detailed level facts or facts that have been aggregated (called summary tables)
  - From modeling standpoint, primary key of the fact table is usually a composite key that i made up of all of its foreign keys

- Dimension Tables:
  - Lookup or reference Tables
  - Contain relatively static data
  - Store infomration you normally use to contain queries
  - usually textual and descriptive and you can sue them as row headers
  - Structure, often composed of one or more hierarchies, that categorized data.
  - Dimensional attributes help describe the dimensional value, they are normally descriptive, textual values. Serveral disnct dimensions combined with facts enable you to answer business questions.
  - Dimension data is typically collected at the lowest level of dtail and the naggregated into higher lvel totals that are more useful.
  - Hiearchies:
    - Natural rollups or aggrgations. Logical structure that use ordered levels to organize data. Can be useed to define data aggregation
    - Each levle is logically connected to the levels above and below it
    - Example: product dimension can have two hierarchies, one for product categories and one for product suppliers
  - Levels:
    - level represent a position in a hierarchy
    - Relationships: Top-to-bottom ordering, most general(root) to most specific information
- Physical Design:
  - Physical (tables, indexes, integrity constraints - primary, foreign key, not null)
  - maerialized views
  - dimensions
  - columns
  - Must map entities to tables, relationships to foreign key constraints
  - attribtes to columns
  - primary unique identifiers to primary key
  - unique identifiers to unique key constraints
  - Tablespaces:
    - consists of one or more datafiles, datafile is associated with only one tablespace
    - Should represent logical business units if possible
    - Partition large tables improves performance because it is more manageable, typically partition based on transaction dates. Fore example, each month,
  - Improve memory by table compression, heap-organized tables. Can enable compression at the tablespace table or partition level. Leads to better scalability and query performance.
    - compressed tables or partitions are updateable but has overhead. high update activity may work against compression by causing some space to be wasted
  - Views
    - Tailored presentation of the data contained in one or more tables or other views.
    - Takes output of a query and treats it as a table
    - Does not require any additional space in database.
  - Integrity Constraints
  - INdexes and Partitioned indexes
    - Bitmap indexes are very common and used to optimized index structures for set-oriented operations. Needed for some optimized data access method such as start transformation
  - Materialized Views
    - query results that have been stored in advance so long-running calculations are not necessary when you actually execute your SQL statements.
  - Dimensions:
    - schema object that defined hierachical relationships between columns or column sets. It is a container of logical relationships and does not require any space in the database.
- Partitioning in Data Warehouses
  - partition pruning
  - parition-wise joins
- Parallel Execution
  - Improves processing for:
    - Queries requiring large table scans, joins, partitioned index scans
    - Creations of large indexes
    - Creation of large tables (including materialized views)
    - Bulk inserts, updates, merges, and deletes
    - When to implement:
      - Symmetric multiprocessors, clusters, or parallel systems
      - Sufficient i/o bandwidth
      - underutilized or intermittently used CPUs
      - Sufficient memory to support additional memory-intensive processes such as sorts, hashing, i/o buffer
    - When to not use it:
      - Short transaction or query
      - CPU, memory, i/o resources are heavily utilized
- Bitmap indexes in Data Warehouses
  - BItmap indexes are most effective for queries that contain multiple conditions in the WHERE clause
  - bitmap join index, which is a bitmap index for the join of two or more tables
  - bitmap compression
- Materialized Views - One technique to improve perforamnce is the creation of summaries. Sumarries are special types of aggregate views that improve query execution times by precalculating expensive joins and aggregation operations prior to execution and storing the results in a table in the database. - Improves performance or providing replicated data - Components of Summary Management: - Mechanisms to define materialized views and dimensions - A refresh mechanism to ensure that all materialized views contain the latest data - Query rewrite capabilityh to transparently rewrite a query to use a materialized view - Guide line to create views: - The purpose of a materialized view is to increase query execution performance - 1. Dimensions should either be denormalized or the joins between tables in a normalized or partially normalized dimension should guarantee that each chidl-side row joins with exactly one parent-side row - 2. If dimensions are denormalized/partially, hierarchical integrity must be maintained beyween the key columns of the dimension tables. - Fact and dimension tables should guarantee that each fact table row joins with exactly one dimension table row - Incremental loads of your detail data should be done using the SQL loader direct-path - Range/composite partition your tables by a monotonically increase time column if possible (DATE type) - After each. load and before refreshing your materialized view, use the VALIDATE_DIMENSION package to incrementally verify dimensional integrity - Table Space
  Dimensional Modeling:
- Consistency/ Quality (Regulatory) - Core Business Data
- Governance
- Verifiability
- Supportability
- Usability
- History
-

OLTP (Transactional Database Design)

- OLTP Performance is about inserting and updating quickly
- Locking must be miimized
- Very small sets of data is retrieved in a query
- Data Consistency is Critical
- Normalization

Warehouse (OLAP):

- Copy of OLTP Data
- Performance is about retrieving the data quickly
- Locking is not an issue
- Large sets of dta are retrieved in a query
- Insert and Update speed is not important

Dimensional Modeling is a design technique for databases intended to support end-users queries in a data warehouse

Steps of Dimensional Modeling:

1. Choose the business process
   1. Basic design build on the actual business process
2. Declare the grain
   1. The exact description of what the dimensional modeling should be focusing on
3. Identify the dimensions
   1. Dimensions must be defined within the grain from the second step of the 4-step process. Dimensions are typically nouns like data, store etc
4. Identify the facts 1. After defining the dimensions, the next step in the process is to make keys for fact table.
   Seven W’s of Data Warehouse Design - User Story:

- How
- What
- When
- Where
- Who
- How many —> fact, rest dimension
- Why

What happens when dimension values change?

- Type 1:
  - Simply overwrite the existing dimenson data with new info
  - Pro: Easiest to implement, Con: Lose ability to see how data looked previously
- Type 2:
  - We keep all historical values of the dimension
    - Better ability to report accurately historically
    - Con: Most complex to implement
- Type 3:
  - We keep prior value and the new value

Goals for Data Warehouses:

- Must make information easily accessible
- Miust present information consistently
- Must adapt to change
- Must present information in a timely way
- Must secure bastion that protects information assets
- Must serve as an authoritative and trustworthy foundation for improved decision making
  PResenting information consistently, timelines, and adaptability all are achieve in ETL processes.

Data Modeling is required to achieve accessibility.
Should not expect end-users will be writing complex outer joins, grouped subqueries, and etc

ETL Design

1. Get data to application and put into a raw database
2. Staging Database, raw data mapping to staging
3. QA Database
4. Logging Table
