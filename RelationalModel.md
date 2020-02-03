# The Relational Model

- [The Relational Model](#the-relational-model)
  - [Relations](#relations)
    - [Keys](#keys)
      - [Primary Keys](#primary-keys)
      - [Foreign Keys](#foreign-keys)
    - [Data Redundancy](#data-redundancy)
      - [Functional Dependencies](#functional-dependencies)
    - [Association Tables](#association-tables)
  - [Relational Algebra](#relational-algebra)
    - [Projection and Restriction](#projection-and-restriction)
      - [Projection](#projection)
      - [Restriction](#restriction)
    - [Set Operations](#set-operations)
      - [Union](#union)
      - [Difference](#difference)
      - [Intersection](#intersection)
    - [Cartesian Product](#cartesian-product)
    - [Join](#join)
      - [Inner Join](#inner-join)
      - [Outer Join](#outer-join)
      - [Natural Join](#natural-join)
    - [Aggregation](#aggregation)
      - [Partition](#partition)
      - [Apply Aggregate Functions](#apply-aggregate-functions)
      - [MapReduce](#mapreduce)
  - [Relational Data Model](#relational-data-model)
    - [Database integry constraints](#database-integry-constraints)
      - [Entity integrity](#entity-integrity)
      - [Referential integrity](#referential-integrity)
    - [Database normalisation](#database-normalisation)
      - [First normal form (1NF)](#first-normal-form-1nf)
      - [Second normal form](#second-normal-form)
      - [Third normal form](#third-normal-form)

## Relations
Simplified, a _relation_ is a table in which data is placed. In a table, a single row represents a single object while all the rows represent objects of the same kind--for example, a relation could be imagined as a _basket of apples_. The order of rows is irrelevant.

A _relation_ in a database table is referred to as a _table_ in relational terminology. A _tuple_ in a database table is equivalent to a _row_ in relational model terminology. Other terms for tuple/row are _n-tuple_, _record_, and _vector_.

An _attribute_ in a database table is equivalent to a column in relational model terminology. 

A _domain_ describes the set of possible values for a given attribute, and is roughly equivalent to the _column type_ in database terminology (eg. the domain of a theoretical _colour_ attribute could be `{red, green, blue}`).

### Keys
A relation cannot contain two identical tuples. For any given relation, a key is the minimum set of attributes that uniquely identify a tuple. Every set that fits these criteria is considered a _candidate key_.

#### Primary Keys

In a database, it's necessary to designate a _primary key_ from the candidate keys. A primary key identifies a tuple in a relation. A primary key should satisfy the following criteria:

1. Brevity
2. Simplicity
3. Context

It's also possible to create an _artificial key_, which is an attribute that is added to the relation without having any real meaning in the domain being modeled; its sole purpose is to uniquely identify the tuples of the relation (ie. it's created for no other reason than to differentiate and identify the tuples).

Artificial keys are common in databases. Key reasons for this are _scalability_ and _processing-time_. Non-artificial keys are often sub-optimal when querying an RDBMS.

Keys can (obviously) never be `NULL`.

#### Foreign Keys
It is extremely rare for a database to contain only one table. For example, for a table `apple` the table `variety` would list various known apple varieties. Because an apple belongs to a certain variety, there is a relationship between these two tables.

This is implemented using _foreign keys_. A foreign key is a set of one or more columns in a table that reference the primary key of another table. If the primary key being referenced consists of multiple columns, the foreign key must have an equal amount of columns.

### Data Redundancy
Storing information in more than one place is known as _data redundancy_, which is an ill-advised practice. If data is not updated everywhere at the same time (itself a nightmarish concept), the result would be data inconsistencies and errors.

In a relation, if an attribute `A` depends uniquely on a group of attributes `G`--and if `G` is not a candidate key--it is possible to create a new relation that will contain the attributes of both `A` and `G`. However, `G` must be the minimum set, ie. an attribute cannot be removed from `G` without destroying the dependency relationship `A`-`G`.

`G` will be a candidate key for the new relation.

If there are any other attributes `B` that--like `A`--also uniquely depend on `G`, then these will also have to be moved to the new relation.

#### Functional Dependencies
Formally, if column `A` of a table uniquely identifies the column `B` of the same table then `B` is functionally dependent on `A` (`A->B`).

### Association Tables
Cardinality is _the number of elements in a set or a grouping, as a property of that grouping._

A number of cardinalities are possible:
- **One-to-many** (one apple for multiple people)
- **Many-to-one** (multiple apples for one person)
- **Many-to-many** (multiple apples for one person and multiple people for one apple)
- **One-to-one** (one apple for one person)

To represent many-to-many relationships between two objects we create _association tables_. Association tables consist of at least two foreign keys, each of which references one of the two objects.


## Relational Algebra
Everything having to do with the manipulation of relations is called _relational algebra_.

### Projection and Restriction
Projection and restriction are both _unary_ operations, meaning they are defined on a single relation.

#### Projection
Projection means choosing the attributes of a relation we want to display and excluding whatever attributes are left over.

#### Restriction
Projection is choosing a sub-set of all available columns; restriction is choosing a sub-set of all available _rows_. Restriction can be thought of as a filter that selects rows meeting a specified condition.

### Set Operations
Set operations are _binary_, ie. they combine two or more elements to produce a third. In relational algebra, set operators operate on pairs of relations contained within the same schema--that is, set operations operate on a _set of relations_.

#### Union
The union of two relations of the same schema `R1` and `R2` produces a third relation of the same schema containing all tuples of `R1` and `R2`.

`R1 union R2 = R3`

#### Difference
The difference between an `R3` and `R2` relation results in an `R1` relation containing all tuples of `R3` that are not found in `R2`.

`R3 difference R2 = R1`

The difference between `R3` and `R2` does not produce the same result as the difference between `R2` and `R3`. The difference operation is _not commutative_. 

It's possible to calculate the difference between `R1` and `R2` even if `R2` contains tuples that are not found in `R1`.

#### Intersection
The intersection between two relations `R1` and `R2` results in a third relation `R3` containing only the tuples that are found in both `R1` and `R2`.

Intersection is equivalent to two consecutive `difference`s. `R1 intersction R2` == `R1 difference (R1 difference R2)`.

### Cartesian Product
The _Cartesian product_ of two relations `R1` and `R2` represents all of the possible combinations of `R1` tuples and `R2` tuples. 

Given a table representing three varieties of apples and a table representing four tasters, the Cartesian product will produce a table that:
- contains the columns of `R1` and the columns of `R2`
- has rows for all of the apples supplied for tasting

Ie. the Cartesian product between a relation with 3 tuples and a relation with 4 tuples results in a relation of 12 tuples.

### Join
The purpose of `JOIN` is to "combine" two or more tables, creating one big table containing information from all combined tables.

#### Inner Join
Generally, when leaving `JOIN` unqualified, we mean `INNER JOIN`.

Formally, we perform an `INNER JOIN` on the tables `T1` and `T2` according to the condition `T2.[FK]T1Id = T1.[PK]Id`. The join condition must always be specified. In a `JOIN`, the foreign key of one table references a candidate key of another table (often the primary key). 

A primary key can consist of many columns. In this case, a join condition could look like this:

`R1.Attr1 = R2.Attr3 AND R1.Attr2 = R2.Attr4`

Given the two tables `Customer` and `Order`, with the latter having a `CustomerId` foreign key referencing `Customer`'s primary key `Id`, an inner join will return a list of all orders together with their respective customers. However, it will not return customers with no order. 

An `INNER JOIN` will only return results when there is a record in the table on both sides of the join.

#### Outer Join
An `OUTER JOIN` can return rows from one table even when there is no corresponding row in the other table. Building on the example from the last section, it can return all customers together with any orders they've placed, including customers who haven't placed any orders.

A `LEFT JOIN` returns all rows from the left table, even if there are no matches in the right table. That is to say, if the `ON` clause matches zero records in the right table, the join will still return a row in the result, with `NULL` in each column from the right table.

For `RIGHT JOIN`, reverse the above. 

`FULL JOIN` combines the results of both left and right outer joins. The joined table will contain all records from both tables, with `NULL` for missing matches on both sides.

#### Natural Join
A _natural join_ is a regular join. If the two relations being joins have columns that appear in both relations (ie. exactly the same name in both tables), the join can be implicit. 

### Aggregation
An aggregate is a whole formed by combining several separate elements. In a database context, aggregration is used to perform computations over multiple rows of a table. A proper aggregation consists of two steps/elements:

1. A group of *partitioning attributes* (eg. _color_)
2. One or more *aggregate function(s)* (eg. _average_)

#### Partition
If apples are partitioned by colour, groups of apples (aggregates) are formed, and the apples within each group all have the same colour. 

Formally, the purpose of partitioning is to create groups of rows such that all rows in each group have the same values for the partitioning attributes.

Partitioning by multiple attributes is also possible. 

#### Apply Aggregate Functions
The role of aggregate functions is to take a group of multiple rows and perform a calculation on them, finally returning a single value for each group. An example is calculating the average weight of each group of apples.

Generally, an aggregate function takes a list and returns a single value. It's also possible for aggregate functions to take the values of multiple attributes (ie. multiple lists).

#### MapReduce
When a data set is especially large, several servers can be used to process the data in parallel, with one server coordinating the others and distributing the data among its slaves.

With `MapReduce`, the information is first divided into chunks. Each chunk has a key and a value. Next a `reduce` function is defined. When the calculation is executed, all chunks with a particular key are directed to the same server, and this server then applies the reduce function to all of the values of the chunks it receives.

| MapReduce      | Aggregation                    |
| -------------- | -----------------------------  |
| Data chunk     | Single row                     |
| Key            | Patritioning attribute         |
| Value          | Attributes sent to aggr. func. |
| _Reduce_ func. | Aggregate function             |

## Relational Data Model
The _relational data model_ makes sure data is entered and/or updated in one place only, containing a number of linked tables that provide access to all data related to a particular record, or set of records.

_Entities_ are single items that can be described. They are generally written with a capital letter, and translate into database tables. An _entity instance_ is a single occurrence of an entity and equivalent to one record in the database._Entity types_ are collections of two or more tables with similar attribute that often describe very similar or related data.

### Database integry constraints
#### Entity integrity
_Entity integrity_ defines the rules for primary key values. The primary key (and/or any component(s) that make it up) can't be null, and the DBMS will not allow any operation that produces a PK that violates this rule. 

#### Referential integrity
_Referential integrity_ defines the rules for how foreign keys are maintained; the FK must reference an existing primary key or be `null`. The DBMS has rules to PK values from being changed without also changing the FK value that matches it. No **update** or **delete** operation can be performed on a primary key field if the foreign key value exists, and likewise an **insert** or **update** operation would be prevented if it'd result in a situation with no matching PK--this prevents _orphan records_. 

The only exception to these DBMS restrictions would be constraining a foreign key field to not contain `null`s.

### Database normalisation
One of the primary concepts of relational databases is maintaining a single source of truth, ie. storing, updating, and manipulating data in one place only. We normalise databases for two key reason:
1. To eliminate data redundancy
2. To ensure logical entities (ie. all related data is stored logically and together)

A non-normalised database could contain duplicate data in multiple locations, resulting in records with poor reliability and accuracy, giving us an inconsistent system no-one would want to use. Non-normalised tables also take up more storage space, and clutter existing tables with unnecessary table entries.

#### First normal form (1NF)
1NF eliminates repeating groups, meaning that multi-valued attributes are removed. 1NF also establishes primary keys, thereby uniquely identifying records in a table.

1NF exists if these two criteria are met:
1. A primary key has been defined to uniquely identify records
2. There are no repeating groups in the relation

Or, from [Microsoft](https://support.microsoft.com/en-gb/help/283878/description-of-the-database-normalization-basics):
- Eliminate repeating groups in individual tables (ie. fields should contain atomic values only)
- Create a separate table for each set of related data.
- Identify each set of related data with a primary key.

**Movies (with repeating groups)**
| Title | Format |
| ----- | ----- |
| Alien | VHS, DVD |
| LotR  | VHS, DVD, BluRay |
| Godzilla | DVD, HD-DVD |

**Movies (1NF)**
| PK Id | Title | FK FormatId|
| ----- | ----- | ----- |
| 1 | Alien | 1 |
| 2 | Alien | 2 |
| 3 | LotR | 1 |
| 4 | LotR | 2 |
| 5 | LotR | 4 |
| 6 | Godzilla | 2 |
| 7 | Godzilla | 3 |

**Format (1NF)**
| PK Id | Format |
| ----- | ------ |
| 1     | VHS    |
| 2     | DVD    |
| 3     | HD-DVD |
| 4     | BluRay |


#### Second normal form
2NF exists if these two criteria are met:
1. The relation is in 1NF
2. Partial dependencies are removed (ie., records should not depend on anything other than a table's primary key)

Again from [Microsoft](https://support.microsoft.com/en-gb/help/283878/description-of-the-database-normalization-basics):
- Create separate tables for sets of values that apply to multiple records.
- Relate these tables with a foreign key.

**Orders (with partial dependencies)**

| OrderId | OrderDate | CustomerId | LastName | Address | BookId | Title | Price |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |

Here, the book information (BookId, Title, Price) are only partially dependent on the primary key and could be associated with multiple orders.

**Orders (2NF)**

| OrderId | OrderDate | CustomerId | LastName | Address |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |

**Book (2NF)**
| BookId | Title | Price |
| ----- | ----- | ----- |

**BookOrderQuantity (2NF)**
| OrderId | BookId | Quantity |
| ----- | ----- | ----- |

(Note: Book and BookOrderQuantity can't be normalised further and are thus actually in 3NF.)

#### Third normal form
3NF exists if these two criteria are met:
1. The relation is in 2NF
2. Transitive dependencies are removed (ie. values that are dependent not on the PK of the table, but rather on another field or fields)

And, yet again from [Microsoft](https://support.microsoft.com/en-gb/help/283878/description-of-the-database-normalization-basics): 
- Eliminate fields that do not depend on the key.

**Orders (with transitive dependencies)**
| OrderId | OrderDate | CustomerId | LastName | Address 
| ----- | ----- | ----- | ----- | ----- |


**Orders (3NF)**
| OrderId | OrderDate | CustomerId |
| ----- | ----- | ----- |


**Customers (3NF)**
| CustomerId | LastName | Address |
| ----- | ----- | ----- |