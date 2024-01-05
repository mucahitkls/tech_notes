SQLAlchemy is a library that facilitates the communication between Python programs and databases. Most of the time, this library is used as an Object Relational Mapper (ORM) tool that translates Python classes to tables on relational database and automatically converts function calls to SQL statements.

SQLAlchemy provides a standard interface that allows developers to create database-agnostic code to communicate with a wide variety of database engines.

SQLAlchemy relies on common design patterns (like Object Pools) to allow developers to create and ship enterprise-grade, production-ready applications easily. Besides that , with SQLAlchemy, boilerplate code to handle tasks like database connections is abstracted away to let developers focus on business logic.

The following sections will introduce important concepts that are needed to be known before dealing with SQLAlchemy applications.

## Python DBAPI	

The Python DBAPI (an acronym for DataBase API) was created to specify how Python modules that integrate with database should expose their interfaces. We won't interact with this API directly - we will use SQLAlchemy as a facade to it - it's good to know that it defines how common functions like `connect`, `close`, `commit`, and `rollback` must behave. Consequently, whenever we use a Python module that adheres to the specification, we can rest assured that we will find these functions and that they will behave as expected.

## SQLAlchemy Engines

Whenever we want to use SQLAlchemy to interact with a database, we need to create an `Engine`. Engines, on SQLAlchemy, are used to manage two crucial factors. `Pools` and `Dialects`. SQLAlchemy uses them to interact with DBAPI functions.

To create an engine and start interacting with databases, we have to import the `create_engine` function from the `sqlalchemy` library and issue a call to it:

```
from sqlalchemy import create_engine
engine = create_engine('postgresql://user:password@localhost:5432/sqlalchemy')
```

This example creates a PostgreSQL engine to communicate with an instance running locally on port `5432` (the default one). It also defines that it will use `user` and `password` as the credentials to interact with the `sqlalchemy` database. Note that, creating an engine does not connect to the database instantly. This process is postponed to when it's needed (like when we submit a query, or when create/update a row in a table).

Since SQLAlchemy relies on the DBAPI specification to interact with databases, the most common database management systems available are supported. PostgreSQL, MySQL, Oracle, Microsoft SQL Server, and SQLite are all examples of engines that we can use alongside with SQLAlchemy.

## SQLAlchemy Connection Pools

[[Connection pooling]] is one of the most traditional implementations of the object pool pattern. Object pools are used as caches of pre-initialized objects ready to use. That is, instead of spending time to create objects that are frequently needed (like connections to databases) the program fetches an existing object from the pool, uses it as desired, and puts back when done.

The main reason why programs take advantage of this design pattern is to improve performance. In the case of database connections, opening and maintaining new ones is expensive, time-consuming, and wastes resources. Besides that, this pattern allows easier management of the number of connections that an application might use simultaneously.

There are various implementations of the connection pool pattern available on SQLAlchemy. For example, creating an Engine through the `create_engine()` function usually generates a QueuePool. This kind of pool comes configured with some reasonable defaults, like a maximum pool size of 5 connections.

As usual production-ready programs need to override these defaults (to fine-tune pools to their needs), most of the different implementations of connection pools provide a similar set of configuration options. The following list shows the most common options with their descriptions:
`pool_size` : Sets the number of connections that the pool will handle.
`max_overflow` : Specifies how many exceeding connections (relative to `pool_size`) the pool supports.
`pool_recycle` : Configures the maximum age (in seconds) of connections in the pool.
`pool timeout` : Identifies how many seconds the program will wait before giving up on getting a connection from the pool.

## SQLAlchemy Dialects

As SQLAlchemy is a facade that enables Python developers to create applications that communicate to different database engines through the same API, we need to make use of [[Dialects]]. Most of the popular relational databases available out there adhere to the SQL (Structured Query Language) standard, but they also introduce proprietary variations. These variations are the solely responsible for the existence of dialects.

For example, let's say that we want to fetch the first then rows of a table called `people`. If our data was being held by a Microsoft SQL Server database engine, SQLAlchemy would need to issue the following query:

```SELECT TOP 10 * FROM people;```

But, if our data was persisted on MySQL instance, then SQLAlchemy would need to issue:

```SELECT * FROM people LIMIT 10;```

Therefore, to know precisely what query to issue, SQLAlchemy needs to be aware of the type of the database that it is dealing with. This is exactly what Dialects do. They make SQLAlchemy aware of the dialect it needs to talk.

On its core, SQLAlchemy includes the following list of dialects:
> Firebird
> Microsoft SQL Server
> MySQL
> Oracle
> PostgreSQL
> SQLite
> Sybase

Dialects for other database engines, like Amazon Redshift, are supported as external projects but can be easily installed.

## SQLAlchemy ORM

ORM, which stands for *Object Relational Mapper*, is the specialization of the *Data Mapper* design pattern that addresses relational databases like MySQL, Oracle and PostgreSQL. *Mappers* are responsible for moving data between objects and a database while keeping them independent of each other. As object-oriented programming languages and relational databases structure data on different ways, we need specific code to translate from one schema to the other.

For example, in a programming language like Python, we can create a `Product` class and an `Order` class to relate as many instances as needed from on class to another (i.e. `Product` can contain a list of instances of `Order` and vice-versa). Though, on relational databases, we need three entities (tables), one to persist products, another one to persist orders, and a third one to relate (through foreign key) products and orders.

SQLAlchemy ORM is an excellent *Data Mapper* solution to translate Python classes into/from tables and to move data between instances of these classes and rows of these tables.

## SQLAlchemy Data Types

While using SQLAlchemy, we can rest assured that we will get support for the most common data types found in relational databases. For example, Booleans, Dates, Times, Strings, and Numeric values are a just a subset of the types that SQLAlchemy provides abstractions for. Besides these basic types, SQLAlchemy includes support for a few vendor-specific types (like JSON) and also allows developers to create custom types and redefine existing ones.

To understand how we use SQLAlchemy data types to map properties of Python classes into columns on a relation database table, let's analyze the following example:

```
class Product(Base):
	__tablename__ = 'products'
	id = Column(Integer, primary_key=True)
	title = Column('title', String(32))
	in_stock = Column('in_stock', Boolean)
	quantity = Column('quantity', Integer)
	price = Column('price', Numeric)
```

In the code snipped above, we are defining a class called `Product` that has six properties. Let's take a look at what these properties do:

+ The `__tablename__` property tells SQLAlchemy that rows of the `products` table must be mapped to this class.
+ The `id` property identifies that this is the `primary_key` in the table and that its type is `Integer`.
+ The `title` property indicates that a column in the table has the same name of the property and that its type is `String`.
+ The `in_stock` property indicates that a column in the table has the same name of the property and that its type is `Boolean`.
+ The `quantity` property indicates that a column in the table has the same name of the property and that its type is `Integer`.
+ The `price` property indicates that a column in the table has the same name of the property and that its type is `numeric`.

Seasoned developers will notice that (usually) relational databases do not have data types with these exact names. SQLAlchemy uses these types as generic representations to what databases support and use the dialect configured to understand what types they translate to. For example, on a PostgreSQL database, the `title` would be mapped to a varchar column.

## SQLAlchemy Relationship Patterns

Let's learn how to use SQLAlchemy to map relationships between classes to relationships between tables.
SQLAlchemy supports four types of relationships: One To Many, Many To One, One to One, and Many to Many.

The first type, *[[One To Many]]*, is used to mark that an instance of a class can be associated with many instances of another class. For example, on a blog engine, an instance of the `Article` class could be associated with many instances of the `Comment` class. In this case, we would map the  mentioned classes and its relations as follows:

```
class Article(Base):
	__tablename__ = 'articles'
	id = Column(Integer, primary_key=True)
	comments = relationship("Comment")

class Comment(Base):
	__tablename__ = 'comments'
	id = Column(Integer, primary_key=True)
	article_id = Column(Integer, ForeignKey('articles.id'))
```

The second type, *[[Many To One]]*, refers to the same relationship described above but from the other perspective. To give a different example, let's say that we want to map the relationship between instances of `Tire` to an instance of a `Car`. As many tires belong to one car and this car contains many tires, we would map this relation as follows:

```
class Tire(Base):
	__tablename__ = 'tires'
	id = Column(Integer, primary_key=True)
	car_id = Column(Integer, ForeignKey('cars.id'))
	car = relationship('Car')

class Car(Base):
	__tablename__ = 'cars'
	id = Column(Integer, primary_key=True)
```

The third type, *[[One To One]]*, refers to relationships where an instance of a particular class may only be associated with one instance of another class, and vice versa. As an example, consider the relationship between`Person` and a `MobilePhone`. Usually, one person possesses one mobile phone and this mobile phone belongs to this person only. To map this relationship on SQLAlchemy, we would create the following code:

```
class Person(Base):
	__tablename__ = 'people'
	id = Column(Integer, primary_key=True)
	mobile_phone = relationship("MobilePhone", uselist=False, back_populates="person")

class MobilePhone(Base):
	__tablename__ = 'mobile_phone'
	id = Column(Integer, primary_key=True)
	person_id = Column(Integer, ForeignKey('people.id'))
	person = relationship("Person", back_populates="mobile_phone")

```

In this example, we pass two extra parameters to the `relationship` function. The first one, `uselist=False`, makes SQLAlchemy understand that `mobile_phone` will hold only a single instance and not an array (multiple) of instances. The second one, `back_populates`, instructs SQLAlchemy to populate the other side of the mapping.

The last type supported by SQLAlchemy, *[[Many To Many]]*, is used when instances of a particular class can have zero or more associations to instances of another class. For example, let's say that we are mapping the relationship of instances of `Student` and instances of `Class` in a system that manages a school. As many students can participate in many classes, we would map the relationship as follows:

```
students_classes_association = Table('students_classes', Base.metadata, 
	Column('student_id', Integer, ForeignKey('students.id')),
	Column('class_id', Integer, ForeignKey('classes.id'))
	)

class Student(Base):
	__tablename__ = 'students'
	id = Column(Integer, primary_key=True)
	classess = relationhip('Class', secondary=students_classes_association)

class Class(Base):
	__tablename__ = 'classes'
	id = Column(Integer, primary_key=True)
```

In this case, we had to create a helper table to persist the association between instances of `Student` and instances of `Class`, as this wouldn't be possible without an extra table. Note that SQLAlchemy aware of the helper table, we passed it in the `secondary` parameter of the `relationship` function.

The above code snippets show just a subset of the mapping options supported by SQLAlchemy. In the following sections, we are going to take a more in-depth look into each one of the available relationship patters.

## SQLAlchemy ORM Cascade

Whenever rows in a particular table are updated or deleted, rows in other tables might need to suffer changes as well. These changes can be simple updates, which are called cascade updates, or full deletes, known as cascade deletes. For example, let's say what we have a table called `shopping_carts`, a table called `products`, and a third one called `shopping_carts_products` that connects the first two tables. If, for some reason, we need to delete rows from `shopping_carts` we will need to delete the related rows from `shopping_carts_products` as well. Otherwise we will end up with a lot of garbage and unfulfilled references in our database.

To make this kind of operation easy to maintain, SQLAlchemy ORM enables developers to map cascade behavior when using relationship() constructs. Like that, when operations are performed on *parent* objects, child objects get updated/deleted as well. The following list provides a brief explanation of the most used cascade strategies on SQLAlchemy ORM:

+ `save-update`: Indicates that when a parent object is saved/updated, child objects are saved/updated as well.
+ `delete`: Indicates that when a parent object is deleted, children of this object will be deleted as well.
+ `delete-orphan`: Indicates that when a child object loses reference to parent, it will get deleted.
+ `merge`: Indicates that `merger()` operations propagate from parent to child.

## SQLAlchemy Sessions

Sessions, on SQLAlchemy ORM, the implementation of the Unit of Work design pattern. A Unit of Work is used to maintain a list of objects affected by a business transaction and to coordinate the writing out of these changes. This means that all modifications tracked by Sessions (Units of Works) will be applied to the underlying database together, or none of them will. In other words, Sessions are used to guarantee the database consistency.

```
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# create an engine
engine = create_engine('postgresql://usr:pass@localhost:5432/sqlalchemy')

# create a configured "Session" class
Session = sessionmaker(bind=engine)

# create a Session
session = Session()
```

As we can see from the code snippet above, we only need one step to get sessions. We need to create a session factory that is bound to the SQLAlchemy engine. After that, we can just issue calls to this session factory to get our sessions.