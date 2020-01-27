---
layout: post
title: Active Record and some cool articles
published: true
---

[Active Record](https://www.martinfowler.com/eaaCatalog/activeRecord.html) is one of the core pieces of Rails, but it originates from the book "Patterns of Enterprise Application Architecture" by Martin Fowler.

From the book, we find that an Active Record is defined as "An object that wraps a row in a database table or view, encapsulates the database access, and adds domain logic on that data."

The essence of an Active Record is a "Domain Model" in which the classes match very closely the record structure of an underlying database. Each Active Record is responsible for saving and loading to the database and also for any domain logic that acts on the data. The data structure of the Active Record should exactly match that of the database: one field in the class for each column in the table.

I learned that Active Record has its weaknesses, though Active Record is a good choice for domain logic that isn't too complex (CRUD operations). In Rails, it helps to simplify your logic to adhere to convention. But when business logic is complex, you'll want to use an object's direct relationships, collections, inheritance, etc. In such cases, a [Data Mapper](https://www.martinfowler.com/eaaCatalog/dataMapper.html) would be a better fit, where you have an extra mapper class that acts as a "middleman" of sorts to the database.

```ruby
# app/models/client.rb
class Client < ApplicationRecord
  has_many :billing_codes
  has_many :billable_weeks
  has_many :timesheets, through: :billable_weeks
end
```

Using Active Record is super easy. Using Ruby's class extension syntax, you declare your class as a subclass and add associations to describe the relationships with other Active Record classes. Active Record in Rails also gives you a lot of power through the Rails Console via the Active Record APIs.

```ruby
>> c = Client.new
=> #<Client id: nil, name: nil, code: nil>
>> c = Client.create(name: "Nile River, Co.", code: "NRC")
=> #<Client id: 1, name: "Nile River, Co.", code: "NRC" ...>
>> first_project = Project.find(1)
=> #<Project id: 1 ...>
>> all_clients = Client.all
=> #<ActiveRecord::Relation [#<Client id: 1, name: "Paper Jam Printers",
   code: "PJP" ...>, #<Client id: 2, name: "Goodness Steaks",
   code: "GOOD_STEAKS" ...>]>

>> first_client = Client.first
=> #<Client id: 1, name: "Paper Jam Printers", code: "PJP" ...>
>> Product.last
=> #<Product id: 1, name: "leaf", sku: nil,
   created_at: "2010-01-12 03:34:41", updated_at: "2010-01-12 03:34:41">
```

Rails also logs the SQL commands passed underneath the hood, like so:

```
User Load (0.1ms)  SELECT "users".* FROM "users" ORDER BY "users"."id"
ASC LIMIT 1
CACHE (0.0ms)  SELECT "users".* FROM "users" ORDER BY "users"."id"
ASC LIMIT 1 LIMIT 1
CACHE (0.0ms)  SELECT "users".* FROM "users" ORDER BY "users"."id"
ASC LIMIT 1
```

### Ruby Articles of Interest

[Understanding array.map(&:method)](https://www.brianstorti.com/understanding-ruby-idiom-map-with-symbol/)