- [Overview](#org84a3c7a)
  - [Q1: What is data?](#org8d876fd)
  - [What level are we going to talk about?](#orgfec9c12)
  - [Challenges](#org93834d5)
- [Part I: The conceptual model](#org4f03d5f)
  - [Example A: House Prices](#org6f86c69)
  - [Q2: What does this data look like?](#org365872d)
  - [An informal description of a table](#orgdbed905)
  - [Formal definition of a relation](#orgb8a2a65)
  - [Q3: About relations](#orge5d2b3b)
  - [Operations on relations](#orgffa5dd8)
- [Part II: The logical model](#org3f43805)
  - [Example B: the "survey" database](#org876bd87)
  - [Q4: Using the database](#org8c75cf8)
  - [SQL: The language of the relational algebra](#orge7c06ed)
    - [Single-table queries](#org381ad98)
    - [Q5: Selecting, computing, and grouping](#orgfd847c4)
  - [Q6: Multiple-table queries](#orgba6ce0a)
  - [Recursive queries](#orgc397529)
  - [Questions](#org1b1f690)
- [Part III: Data modelling](#org8f4c815)

<div class="html">
&#x2014; layout: post title: The Relational Model &#x2014;

</div>


<a id="org84a3c7a"></a>

# Overview


<a id="org8d876fd"></a>

## Q1: What is data?

The relational model is interested in modelling *facts*.


<a id="orgfec9c12"></a>

## What level are we going to talk about?

From abstract to concrete:

1.  The *conceptual* level is the mathematics of facts: it is expressed using set-theoretic notions.

2.  The *logical* level describes how the conceptual model is represented and manipulated by a computer. It is the level of SQL, the language with which you interact with the database.

3.  The *physical* level is the "back-end": it describes how the bits are organised within a computer, how your commands are understood and compiled, and the mechanics of computation on data.

We will talk exclusively about the conceptual and logical levels. The main challenge in this subject is that very few existing database systems correctly model the conceptual model.

I'll start with some data and talk about what the logical level might look like, then move to the conceptual, then back to the logical level.


<a id="org93834d5"></a>

## Challenges

-   Most database systems incorrectly implement the conceptual level
-   Most practitioners know only the logical level
-   Much real-world code drags in the physical level where it doesn't belong


<a id="org4f03d5f"></a>

# Part I: The conceptual model


<a id="org6f86c69"></a>

## Example A: House Prices

Look at <data/median-house-price-by-lad.csv>


<a id="org365872d"></a>

## Q2: What does this data look like?

-   What kind of structure is this?
-   What regularities can you see?
-   What anomalies are there?
-   Would you make any structural changes?


<a id="orgdbed905"></a>

## An informal description of a table

1.  The data is written as a *table*, which consists of a *header* and a sequence *rows*.

2.  All the rows have the same structure, as determined by the header: a row is a sequence of values; and the type of each value is determined by the header.

The next section is a formal version of that specfication. Very few real-world databases correctly follow this formal version but it is nonetheless helpful to have it in mind.

The relational model represents facts, of a certain sort. The sort of facts it represents are those of the form \(P(x, y, \dots, z)\) where \(P\) is a *predicate*, ie, either true of false depending on the values asigned to the \(x, y, \dots z\).

*Example*: Looking at our example table, \(H(d, n, p_1, p_2)\) might represent the proposition “the regional district \(d\) has name \(n\), and an average house price in 1996 of \(p_1\) and in 2009 of \(p_2\).”

*Example*: \(P(f, l)\) might mean “there exists a person having first name \(f\) and last name \(l\).”

A relational database represents these facts by explicitly storing the *extension* of such a \(P\); that is, the set of all values of \(x, y, \dots z\) that make the predicate true. For any other values, the proposition is assumed to be false.

The *closed-world assumption:* If some tuple is not in the database, then the corresponding proposition if fasle. The closed-world assumption is frequently violated in real-world databases.


<a id="orgb8a2a65"></a>

## Formal definition of a relation

The following translation is not exact:

| Conceptual | Logical       | Comment           |
|---------- |------------- |----------------- |
| Domain     | Type          |                   |
| Attribute  | Column header |                   |
| Header     | Header        |                   |
| Relation   | Table         | *Header and rows* |
| Body       | Body          | *Set of rows*     |
| Tuple      | Row           |                   |
| Value      | Field, value  |                   |

A *domain* is a finite (!) set. All "values" in the relational model are elements of some particular domain.

We assume the existence of some set of possible "names". (These are usually strings satisfying restrictions that make them suitable for computer variables names.)

An *attribute* is a pair, consisting of a name and a domain.

A *header* is a finite set (possibly empty) of attributes, such that all the names are unique.

A *relation* is a header, together with a subset of the cartesian product of all the domains in the header. This subset is called the *body* of the relation. An element of the body is called a *tuple*.

Thus a tuple is, for every attribute in the header, a value in the domain of that attribute.


<a id="orge5d2b3b"></a>

## Q3: About relations

1.  From the *definition* of relation:
    -   Does order of tuples matter?
    -   Does order of attributes matter?
    -   Is every tuple unique?

2.  Is the example dataset a relation?

3.  How many relations have no attributes? What is/are they? Why would you use them?

4.  Is it possible for a value to be `NULL` or “missing”?

5.  What kinds of operation can you imagine on a single relation?

6.  What kinds of operation can you imagine on one or more relations?


<a id="orgffa5dd8"></a>

## Operations on relations

This is the *relational algebra*.

It's called an algebra because all the operations produce a relation.


<a id="org3f43805"></a>

# Part II: The logical model

| Conceptual | Logical       | Comment           |
|---------- |------------- |----------------- |
| Domain     | Type          |                   |
| Attribute  | Column header |                   |
| Header     | Header        |                   |
| Relation   | Table         | *Header and rows* |
| Body       | Body          | *Set of rows*     |
| Tuple      | Row           |                   |
| Value      | Field, value  |                   |

In real-world datasets, we often have the following:

-   Rows are not unique
-   Column (or row) ordering matters
-   Some values are missing or `NULL`


<a id="org876bd87"></a>

## Example B: the "survey" database


<a id="org8c75cf8"></a>

## Q4: Using the database

1.  Run sqlite (eg, type `sqlite3 survey.db` at the command line)

2.  Try `.help` (note the dot!)

3.  Try:
    -   `.databases`
    -   `.tables`
    -   `.schema`

4.  Set the display mode:
    -   `.mode column`
    -   `.headers on`


<a id="orge7c06ed"></a>

## SQL: The language of the relational algebra

There is a standard, but particular products may or may not adhere to it perfectly. The [sqlite website](https://sqlite.org/lang_select.html) documents the particular syntax understood by sqlite.


<a id="org381ad98"></a>

### Single-table queries

```sql
SELECT col-expr [AS name], ... 
FROM tbl
[ WHERE expr ] 
[ GROUP BY expr [ HAVING expr ]];
```

There's a special syntax for getting all columns: Use `*` instead of a column name.

Note that the result is always a table.


<a id="orgfd847c4"></a>

### Q5: Selecting, computing, and grouping

1.  Select the identifier and the first name from the `Person` table.

2.  As (1) but rename the first name as `first_name`.

3.  Select the first and last dates from `Visited`. (Hint: there are functions `max()` and `min()` which also work on dates.)

4.  How many readings in total were made by each scientist? (Hint: there's a function `count()` which counts rows; you can say `count(*)`.)

5.  Will sqlite return a table with duplicate rows?

6.  What's the average temperature recorded across all sites and dates by all scientists?

7.  What's the average reading for each kind of measurements?


<a id="orgba6ce0a"></a>

## Q6: Multiple-table queries

1.  What's the average (hint: `avg()`) temperature recorded for each site across all visits?

1.  


<a id="orgc397529"></a>

## Recursive queries


<a id="org1b1f690"></a>

## Questions

1.  Questions from the software carpentry article

2.  Divide!

3.  Finding paths in graphs!


<a id="org8f4c815"></a>

# Part III: Data modelling

-   constraints

-   normal forms

1.  DDL

2.  Make a db out of the Excel files,
