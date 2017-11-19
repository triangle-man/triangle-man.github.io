# Overview

## What is data?

  - Facts?
  - Objects?
  - Triples?
  - Pointers?
  - Documents?
  - Hierarchies?

The relational model is interested in modelling *facts*.

## What level are we going to talk about?

From abstract to concrete:

1.  The *conceptual* level is the mathematics of facts: it is expressed
    using set-theoretic notions.

2.  The *logical* level describes how the conceptual model is
    represented and manipulated by a computer. It is the level of SQL,
    the language with which you interact with the database.

3.  The *physical* level is the "back-end": it describes how the bits
    are organised within a computer, how your commands are understood
    and compiled, and the mechanics of computation on data.

We will talk exclusively about the conceptual and logical levels. The
main challenge in this subject is that very few existing database
systems correctly model the conceptual model.

I'll start with some data and talk about what the logical level might
look like, then move to the conceptual, then back to the logical level.

## Challenges

  - Most database systems incorrectly implement the conceptual level
  - Most practitioners know only the logical level
  - Much real-world code drags in the physical level where it doesn't
    belong

# Example I: House Prices

Look at
[file:data/median-house-price-by-lad.csv](data/median-house-price-by-lad.csv)

FIXME: Possibly use Looked-after children data

## Questions to think about

  - What kind of structure is this?
  - What regularities can you see?
  - What anomalies are there?
  - Would you make any structural changes?

## An informal description

1.  The data is written as a *table*, which consists of a *header* and a
    sequence *rows*.

2.  All the rows have the same structure, as determined by the header.

The next section is a formal version of that specfication. Very few
real-world databases correctly follow this formal version but it is
nonetheless helpful to have it in mind.

# The conceptual model

The relational model represents facts, of a certain sort. The sort of
facts it stores are those of the form \(P(x, y, z, ...)\) where \(P\) is
a *predicate*, ie, either true of false depending on the values asigned
to the $x, y, …$.

# The logical model

## Recursive queries

## Questions

From the *definition* of relation:

  - Does order of tuples matter/
  - Does order of attributes matter?
  - Is every tuple unique?

How many relations have no attributes? What is/are they? Why would you
use them?

Is it possible for a value to be `NULL`?
