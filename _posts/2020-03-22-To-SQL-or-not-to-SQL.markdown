---
layout: post
title:  "To SQL or not to SQL"
date:   2020-03-22 10:20:12 -0700
categories: sql database nosql
---

My second week at Flatiron has taken me away from the delightful syntax of Ruby, and thrown me into the labyrinth that is SQL.
While I have always understood the value of a database, I've never had to implement or query one before. I quickly realized that there seem to be 'SQL' people and 'non-SQL' people. I also found myself in the latter camp. As I struggled to master the syntax, I began wondering if there was a better database system. I knew there was another way of thinking about databases, broadly called 'NoSQL', and I decided to take a look. That look quickly lead me down the rabbit hole of data organization and persistence.

## Structure

SQL functions much like the physical paper-based ledgers of yore. Data is organized into tables, each with dedicated columns describing the name, type, and even size constraints on what can be entered. Individual objects become rows with entries matching the criteria of each column. Each database can contain many tables, and once their structure is decided upon it becomes the SQL database _schema_. SQL databases can be altered after its creation, but large scale changes can be difficult. Any attempt to enter data that is not in accordance with the schema will be rejected. Each entry is assigned a unique id number, minimizing duplication of data and allowing for relationships between entries to be build directly from the database through queries, instead of offloading that work onto a client application. SQL excels at providing consistent, normalized data.

NoSQL databases take a different approach. Instead of relying upon a rigid predefined schema, most NoSQL implementations rely upon a _document_ model. Data can immediately be entered in the form of JSON style hashes, and both databases and documents can be created on the fly. What's more, instead of relying upon the ledger-style relationships of a SQL database, NoSQL can utilize many different data structures such as graphs, n-trees, column-family, and key-value pairs. The looser constraints of a NoSQL database can allow for duplication (denormalization) of data in order to speed up search queries. No decision is without a tradeoff, however, as denormalized data is much slower to update. NoSQL's flexibility means it can be quickly created, updated, and changed to meet a project's needs, however a poorly designed database will plague a project as it develops.

## Function

Here is an example query from a database storing crowdfunding projects, users, and pledge amounts.

I want all projects and the total amount pledged to them, listed alphabetically by title.

**Example SQL Query**

{% highlight sql %}
  SELECT projects.title, SUM(pledges.amount) 
  FROM projects 
  JOIN pledges ON projects.id = pledges.project_id 
  GROUP BY projects.title;
{% endhighlight %}

The rigidity of SQL leads to some powerful functionality, namely `JOIN`s. Our ledger structure allows for the comparison of two or more related tables, and even some processing of the results (such as `SUM`, `COUNT`, `AVG`, `MAX`, and `MIN`) in one query.

**Example NoSQL (MongoDB) query**

{% highlight json %}
db..group({

   "key":{
         "projects.title": true
   },
   "initial": {
         "sumpledges.amount": 0
   },
   "reduce": function( obj , prev ){

         prev.sumpledges.amount  = prev.sumpledges.amount  + obj.pledges.amount  - 0;

   },
   "finalize": function( prev ){

   },
   "cond": {
	"$where": "this.CT projects.title, SUM(pledges.amount) FROM projects JOIN pledges ON projects.id  == this. pledges.project_id "
   }

});
{% endhighlight %}

Translation performed using this [SQL to MongoDB converter](https://www.site24x7.com/tools/sql-to-mongodb.html).

I have no exposure to MongoDB, but luckily tools exist to convert queries from SQL to NoSQL and vice versa. While it may not be immediately apparent, our example MongoDB query highlights a significant difference between the two: NoSQL has no support for `JOIN`. In addition to the clunky syntax, the same flexibility that let us spin up the database and start creating entries doesn't provide us with the consistency needed to perform such operations at the database level. Instead, our client application will be required to do much of the post-processing to get the desired relationships.

## Use Cases

Conveniently, there are two good acronyms to describe the desirability of either system:

SQL provides ACID compliance:
- *Atomicity*: Transactions with the database will either succeed or fail, there will be no partial results.
- *Consistency*: All data written to the database will be valid.
- *Isolation*: Even if transactions are received simultaneously, they will be processed sequentially.
- *Durability*: Once a transaction is written to the database it is considered permanent, even after a power failure.

NoSQL adheres to the BASE standard:
- *Base Availability*: The database will always have data available, even if it is incorrect or inconsistent.
- *Soft state*: The database will change over time.
- *Eventual consistency*: Eventually, changes will propagate through the system and it will resolve consistently.

I find it somewhat humorous that the acronyms also adhere to the design principles of each system. `SQL == ACID`. `NoSQL ~= BASE`.

Another consideration after standards compliance is scalability. SQL was designed to run on a single server, which will require specific hardware upgrades to increase capacity and performance. NoSQL was built around distributed computing, and can be spread across multiple machines. 

## Conclusion

Ultimately, one style of database is not inherently superior to the other. Each excels in different use cases, and more recent development has focused on convergence. Offerings such as the [MySQL Document Store](https://www.mysql.com/products/enterprise/document_store.html) and the [MongoDB Multi-document ACID Transactions](https://www.mongodb.com/blog/post/mongodb-multi-document-acid-transactions-general-availability) attempt to bring SQL to NoSQL and vice versa. Even the idea that tech stacks like LAMP or MEAN _require_ a certain kind of database is a misconception. It is fundamentally a matter of knowledge and experience. SQL was invented in the 1970s, it has had close to 5 decades to mature and propagate through a variety of industries. Many of the NoSQL implementations have been around for fewer than 10 years. As knowledge and skills in the development community continue to grow, careful consideration of project needs will underpin the choice to SQL or not to SQL.


### Sources
Here are some of the articles that helped me compile this post:

[Sitepoint](https://www.sitepoint.com/sql-vs-nosql-differences/)

[GSPANN](https://www.gspann.com/resources/blogs/sql-vs-nosql-database)

[Thorn](https://www.thorntech.com/2019/03/sql-vs-nosql/)
