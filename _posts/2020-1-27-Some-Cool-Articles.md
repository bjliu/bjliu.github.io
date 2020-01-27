---
layout: post
title: Active Record and some cool articles
published: true
---

# Active Record
Active Record is one of the core pieces of Rails, but it originates from the book "Patterns of Enterprise Application Architecture" by Martin Fowler.

From the book, we find that an Active Record is defined as "An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data."

The essence of an Active Record is a "Domain Model" in which the classes match very closely the record structure of an underlying database. Each Active Record is responsible for saving and loading to the database and also for any domain logic that acts on the data.
The data structure of the Active Record should exactly match that of the database: one field in the class for each column in the table.

An Active Record class typically has methods that do the following:
* Construct an instance of the Active Record from a SQL result set row
* Construct a new instance for later insertion into the table
* Static finder methods to wrap commonly used SQL queries and return Active Record objects
* Update the database and insert into it the data in the Active Record
* Get and set the fields
* Implement some pieces of business logic.

Active Record is a good choice for domain logic that isn't too complex (CRUD operations).
It's easy to build Active Record but their primary problem is that they work well only if the Active Record objects correspond directly to their database tables. When business logic is complex, you'll want to use an object's direct relationships, collections, inheritance, etc. In such cases, a Data Mapper would be a better fit. Active Record also couples the object design to the database design, making it more difficult to refactor either.

# Migrations
Typically, database schema updates can be very complex. Tables are added, names of clumns are changed, things are dropped...and all the time you need to make sure that whatever you do to a database can be rolled back. Rails Active Record Migrations makes it convenient to execute a migration.

# Ruby

[Understanding array.map(&:method)](https://www.brianstorti.com/understanding-ruby-idiom-map-with-symbol/)