Now that we got a better understanding of the most important pieces of SQLAlchemy, it's time to start practicing it. In the following sections, we will create a small project based on `pipenv` - a Python dependency manager - and add some classes to it. Then we will map these classes to tables persisted to a PostgreSQL database and learn how to query data.

## Starting the Project

To create our tutorial project, we have to have Python installed on our machine and `pipenv` installed as a global Python package. The following commands will install `pipenv` and set up the project. These commands are dependent on Python.

```
# install pipenv globally
pip install pipenv

# create a new directory for project
mkdir sqlalchemy-project

# change workign directory to it
cd sqlalchemy-project

# create a Python 3 project
pipenv --three
```


## Running PostgreSQL

It is already mentioned, SQLAlchemy provides support for many different databases engines, but the instructions that follow will focus on PostgreSQL. There are many ways to get an instance of PostgreSQL. One of them is to use some cloud provider like Heroku or ElephantSQL (both of them have free tiers). Another possibility is to install PostgreSQL locally on our environment. A third option is to run a PostgreSQL instance inside a Docker container.

The third option is probably the best choice because it has the performance of an instance running locally, it's free forever, and because it's easy to create and destroy Docker instances. The only (small) disadvantage is that we need to install Docker locally.

After having Docker installed, we can create and destroy *dockerized* PostgreSQL instances with the following commands:

```
# create a PostgreSQL instance
docker run --name sqlalchemy-orm-psql \
	-e POSTGRES_PASSWORD=pass \
	-e POSTGRES_USER=use \
	-e POSTGRES_DB=sqlalchemy \
	-e 5432:5432 \
	-d postgres

# stop instance
docker stop sqlalchemy-orm-psql

# destroy instance
docker rm sqlalchemy-orm-psql
```

The first command, the one that creates the PostgreSQL instance, contains a few parameters that are worth inspecting:

`--name`: Defines the name of the Docker instance.
`-e POSTGRES_PASSWORD`: Defines the password to connect to PostgreSQL.
`-e POSTGRES_USER`: Defines the user to connect to PostgreSQL.
`-e POSTGRES_DB`: Defines the main (and only) database available in the PostgreSQL instance.
`-p 5432:5432`: Defines that the local 5432 post will tunnel connections to the same port in the Docker instance.
`-d postgres`: Defines that this Docker instance will be created based on the official PostgreSQL repository.

## Installing SQLAlchemy Dependencies

We will need two packages: `sqlalchemy` and `psycopg2`. The first dependency refers to SQLAlchemy itself, and the second one, `psycopg2`, is the PostgreSQL driver that SQLAlchemy will use to communicate with the database. To install these dependencies, we will use `pipenv`:

```
# install sqlalchemy and psycopg2
pipenv install sqlalchemy psycopg2
```

This command will download both libraries and make them available in our Python virtual environment. Note that to run the scripts that we are going to create, we first need to spawn the virtual environment shell. That is , before executing `python some_script.py`, we need to execute `pipenv shell`. Otherwise, Python won't be able to find the installed dependencies, as they are just available in our new virtual environment.

## Mapping Classes with SQLAlchemy

After starting the *dockerized* PostgreSQL instance and installing the Python dependencies, we can begin to map Python classes to database tables. In the following, we will map four simple classes that represent movies, actors, stuntmen, and contact details. The following diagram illustrates the entities' characteristics and their relations.

![[Pasted image 20231122213014.png]]

To start, we will create a file called `base.py` in our project and add the following code to it:
```
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

engine = create_engine('postgresql://usr:pass@localhost:5432/sqlalchemy')
Session = sessionmaker(bind=engine)
Base = declarative_base()

```

This code creates:
	- a SQLAlchemy Engine that will interact with our *dockerized* PostgreSQL database,
	- a SQLAlchemy ORM session factory bound to this engine,
	- and a base class for our classes definitions

Now let's create and map the `Movie` class. To do this, let's create a new file called movie.py and add the following code to it:

```
from sqlalchemy import Column, String, Integer, Date
from base import Base

class Movie(Base)
	__tablename__ = 'movies'
	
	id = Column(Integer, primary_key=True)
	title = Column(String)
	release_date = Column(Date)
	
	def __init__(self, title, release_date):
		self.title = title
		self.release_date = release_date
		
```

The definition of this class and its mapping characteristics is quite simple. We start by making this class extend the `Base` class defined in the `base.py` module and then we add four properties to it:

> A `__tablename__` to indicate what is the name of the table that will support this class.
> An `id` to represent the primary key in the table.
> A `title` of type `String`.
> A `release_date` of type `Date`.

The next class that we will create and map is the `Actor` class. Let's create a file called `actor.py` and add the following code to it:

```
from sqlalchmey import Column, String, Integer, Date
from base import Base

class Actor(Base):
	__tablename__ = 'actors'
	
	id = Column(Integer, primary_key=True)
	name = Column(String)
	birthday = Column(Date)
	
	def __init__(self, name, birthday):
		self.name = name
		self.birthday = birthday
```

The definitions of this class is pretty similar to the previous one. The differences are the `Actor` has a `name` instead of a `title`, a `birthday` instead of a `release_date`, and that it points to a table called `actors` instead of `movies`.

As many movies can have many actors and vice-versa, we will need to create a *Many To Many* relationship between these two classes, Let's create this relationship by updating the `movie.py` file as follows:

```
from sqlalchemy import Column, String, Integer, Date, Table, ForeignKey
from sqlalchemy.orm import relationship

from base import Base

movies_actors_association = Table(
	'movies_actors', Base.metadata,
	Column('movie_id', Integer, ForeignKey('movies.id')),
	Column('actor_id', Integer, ForeignKey('actors.id'))

)

class Movie(Base):
	__tablename__ = 'movies'
	
	id = Column(Integer, primary_key=True)
	title = Column(String)
	release_date = Column(Date)
	actors = relationship('Actor', secondary=movies_actors_association)
	
	def __init__(self, title, release_date):
		self.title = title
		self.release_date = release_date

```

The difference between this version and previous one is that:

> we imported three new entities: `Table`, `ForeignKey`, and `relationship`;
> we created a movies_actors_association table that connects rows of `actors` and rows of `movies`;
> and we added the actors property to `Movie` and configured the `movies_actors_association` as the intermediary table.

The next class that we will create is `Stuntman`. Assume that a particular `Actor` will have only one `Stuntman` and this `Stuntman` will work only with this `Actor`.  This means that we need to create the `Stuntman` class and a *One To One* relationship between these classes.
To accomplish that, let's create a file called `stuntman.py` and add the following code to it:

```
from sqlalchemy import Column, String, Integer, Boolean, ForeignKey
from sqlalchemy.orm import relationship, backref

from base import Base

class Stuntman(Base):
	__tablename__ = 'stuntmen'
	
	id = Column(Integer, primary_key=True)
	name = Column(String)
	active = Column(Boolean)
	actor_id = Column(Integer, ForeignKey('actors.id'))
	actor = relationship('Actor', backref=backref('Stuntman', uselist=False))
	
	def __init__(self, name, active, actor):
		self.name = name
		self.active = active
		self.actor = actor

```

In this class, we have defined that the `actor` property references an instance of `Actor` and that this actor will get a property called `stuntman` that is not a list (`uselist=False`). That is, whenever we load an instance of `Stuntman`, SQLAlchemy will also load and populate the `Actor` associated with this stuntman.

The fourth and final class that we will map in our tutorial is `ContactDetails`. Instances of this class will hold a `phone_number` and an `address` of a particular `Actor`, and one `Actor` will be able to have many `ContactDetails` associated. Therefore, we will need to use the *Many To One* relationship pattern to map this association. To create this class and this association, let's create a file called `contact_details.py` and add the following source code to it:

```
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship
from base import Base

class ContactDetails(Base):
	__tablename__ = 'contact_details'
	
	id = Column(Integer, primary_key=True)
	phone_number = Column(String)
	actor_id = Column(Integer, ForeignKey('actors.id'))
	actor = relationship('Actor', backref='contact_details')
	
	def __init__(self, phone_number, address, actor):
		self.phone_number = phone_number
		self.address = address
		self.actor = actor

```

As we can see, creating *Many To One* association is kind of similar to creating a *One To One* association. The difference is that in the latter we instructed SQLAlchemy not to use lists. This instruction ends up restricting the association to a single instance instead of a list of instances.


## Persisting Data with SQLAlchemy ORM

Now that we have created our classes, let's create a file called `insterts.py` and generate some instances of these classes to persist to the database. In this file, let's add the following code:

```
# 1 - imports

from datetime import date

from actor import Actor
from base import Session, engine, Base
from contact_details import ContactDetails
from movie import Movie
from stuntman import Stuntman

# 2 - generate database schema
Base.metadata.create_all(engine)

# 3 - create a new session
session = Session()

# 4 - create movies

bourne_identity = Movie('The Bourne Identity', date(2002, 10, 11))
furious_7 = Moive('Furious 7', date(2015, 4, 2))
pain_and_gain = Movie('Pain & Gain', date(2013, 8, 23))

# 5 - create actors

matt_damon = Actor("Matt Damon", date(1970, 10, 8))
dwayne_johnson = Actor("Dwayne Johnson", date(1972, 5, 2))
mark_wahlberg = Actor("Mark Wahlberg", date(1971, 6, 5))

# 6 - add actors to movies
bourne_identity.actors = [matt_damon]
furious_7.actors = [dwayne_johnson]
pain_and_gain.actors = [dwayne_johnson, mark_wahlberg]

# 7 - add contact details to actors
matt_contact = ContactDetails("415 555 2671", "Burbank, CA", matt_damon)
dwayne_contact = ContactDetails("434 264 2671", "Clendale, CA", dwayne_johnson)
dwayne_contact)2 = ContactDetails("421 444 2323", "Clendale, CA", dwayne_johnson)
mark_contact = ContactDetails("421 33 9428", "Clendale, CA", mark_wahlberg)

# 8 - create stuntmen

matt_stuntman = Stuntman('John Doe', True, matt_damon)
dwayne_stuntman = Stuntman('John Roe', True, dwayne_johnson)
mark_stuntman = Stuntman('Richard Roe', True, mark_wahlberg)

# 9 - persists data

session.add(bourne_identity)
session.add(furious_7)
session.add(pain_and_gain)

session.add(matt_contact)
session.add(dwayne_contact)
session.add(dwayne_contact_2)
session.add(mark_contact)

session.add(matt_stuntman)
session.add(dwayne_stuntman)
session.add(mark_stuntman)

# 10 - commit and close session
session.commit()
session.close()

```

This code is split into 10 sections. Let's inspect them:

1. The first section imports the classes that we created, the SQLAlchemy engine, the Base class, the session factory, and `date` from the `datetime` module.
2. The second section instructs SQLAlchemy to generate the database schema. This generation occurs based on the declarations that we made while creating the four main classes that compose our tutorial.
3. The third section extracts a new session from the session factory.
4. The fourth section creates three instances of `Movie` class.
5. The fifth section creates three instances of the `Actor` class.
6. The sixth section adds actors to movies. Note that the Pain & Gain movie references two actors: Dwayne Johnson and Mark Wahlberg.
7. The seventh section creates instances of the ContactDetails class and defines what actors these instances are associated to.
8. The eighth section defines three stuntmen and also defines what actors these stuntmen are associated to.
9. The ninth section uses the current session to save the movies, actors, contact details, and stuntmen created. Note that we haven't explicitly saved actors. This is not needed because SQLAlchemy, by defaults, uses the `save-update` cascade strategy.
10. The tenth section commits the current session to the database and closes it.

Running this python file will create five tables in the PostgreSQL database and populate these tables with the data that we created. 



## Querying Data with SQLAlchemy ORM

Querying data with SQLAlchemy ORM is quite simple. This library provides an intuitive, fluent API that enables developers to write queries that are easy to read and to maintain. On SQLAlchemy ORM, all queries start with a Query Object that is extracted from the current session and that is associated with a particular mapped class. To this API in action, let's create a file called `queries.py` and add to it the following source code:

```
from actor import Actor
from base import Session
from contact_details import ContactDetails
from movie import Movie

session = Session()

movies = session.query(Movie).all()

print('\n### All movies: ')
for movie in movies:
	print(f"{movie.title} was released on {movie.release_date}")
print('')
```

The code snipped above - to retrieve all movies from the database, we just needed to fetch a session from the session factory, use it to get a query associated with `Movie`, and then call the `all()` function on this query object. The Query API provides dozens of useful function like `all()`. In the following list, we can see a brief explanation about the most important ones:

+ `count()`: Returns the total number of rows a query.
+ `filter()`: Filters the query by applying a criteria.
+ `delete()`: Removes from the database the rows matched by a query.
+ `distinct()`: Applies a distinct statement to a query.
+ `exists()`: Adds an exists operator to a subquery.
+ `first()`: Returns the first row in a query.
+ `get()`: Returns the row referenced by the primary key parameter passed as argument.
+ `join()`: Creates a SQL join in a query.
+ `limit()`: Limits the number of rows returned by a query.
+ `order_by()`: Sets and order in the rows returned by a query.

To explore the usage of some of these functions, let's append following code to the `queries.py` script:

```
from datetime import date

# other imports and sections ...

# 5 - get movies after 15-01-01

movies = session.query(Movie).filter(Movie.release_date > date(2015, 1, 1)).all()

print("### Recent Movies: ")
for movie in movies:
	print(f"{movie.title} was released after 2015")
print('')


# 6 - moives that Dwayne Johnson participated
the_rock_movies = session.query(Movie).join(Actor, Movie.actors)\
	.filter(Actor.name == 'Dwayne Johnson').all()

print("### Dwayne Johnson movies: ")
for movie in movies:
	print(f'The Rock starred in {movie.title}')
print('')


# 7 - get actors that have house in Glendale
glendale_stars = session.query(Actor).join(ContactDetails)\
	.filter(ContactDetails.address.ilike('%glendale%')).all()

print('### Actors that live in Glendale: ')
for actor in glendale_starts:
	print(f"{actor.name} has a house in Glendale")

print('')
```


The fifth section of the updated script uses the `filter()` function to fetch only movies that were released after January the first, 2015. The sixth section shows to use `join()` to fetch instances of `Movie` that the `Actor` Dwayne Johnson participated in. The seventh and last section, shows the usage of `join()` and `ilike()` functions to retrieve actors that have houses in Glendale.

