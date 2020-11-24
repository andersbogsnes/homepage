---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Working with data

---

A core component of an API is data

We want to pass data back and forth to our API using JSON

Data needs to be stored - generally we want a database!

---

## SQLAlchemy ORM

When working with APIs, we generally want to fetch a row at a time or a limited queryset

The SQLAlchemy ORM (**O**bject **R**elational **M**apper) is great for working with rows of data

An ORM represents a row in the database as a Python object and makes it easy to manipulate it

---

First, we need to install sqlalchemy:

```bash
pip install --pre sqlalchemy # For sqlalchemy==1.4
```

:warning: SQLAlchemy 2.0 is around the corner. We want to use `1.4` which has support for `2.0` functionality, but it's still beta

---

## Declaring a Table

SQLAlchemy lets us declare a Table class that represents a row in our database. To do this, SQLAlchemy needs to generate a Base class to inherit from


```python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

---

Now we can create our mapping class

```python
import sqlalchemy as sa

class Listing(Base):
    __tablename__ = "listings"

    id = sa.Column(sa.Integer, primary_key=True)
    username = sa.Column(sa.String)
    email = sa.Column(sa.String)
    address = sa.Column(sa.String)
    price = sa.Column(sa.Float)
```

---

## Note

We don't need to provide an `__init__` method - SQLAlchemy autogenerates one.

You can, however, add an `__init__` if you prefer

---

## Creating our database

We have a mapping, but no database yet - we can ask SQLAlchemy to write the SQL needed to match our declaration

```python
# Set the future flag to opt-in to 2.0 behaviour
engine = sa.create_engine("sqlite:///airbnb.db", future=True)

# All tables inheriting from Base
# are registered in the metadata object
Base.metadata.create_all(engine) # Execute the sql with the engine
```

---
 
## Inserting some data

We can now insert some data into the database using our ORM class

```python
# Make an instance of Listing
listing_1 = Listing(username="Anders",
                    email="andersbogsnes@gmail.com",
                    address="Nørrebrogade 20",
                    price=700)

# The context manager automatically closes the session
with Session(engine) as session:
    session.add(listing_1)
    session.commit()
```

---

## Reading the data

To read the data, we must create a SELECT statement and execute it in a Session

```python
sql = sa.select(Listing).where(Listing.username == "Anders")

with Session(engine) as session:
    # Get the first result
    result = session.execute(sql).first()

>>> print(result.Listing.username)
"Anders"
```

---

### Using scalars

The result we get back from sqlalchemy is a `Row` object with columns for each item in the `select`.

When using the ORM, we generally just want the object, which we can get with `.scalars`

```python
with Session(engine) as session:
    result = session.execute(sql).scalars().first()

>>> print(result.username)
"Anders"
```

---

#### Aside: The commit

When working with inserts, there are two principles we want to follow

- The Session should match the lifetime of the request
- Commit only when you need to

---

##### Session

The Session creates a database connection, which needs to be closed.

<p class="fragment">Lots of opens connections is bad! </p>

<p class="fragment">But having to open a new connection constantly is also bad...</p>

<p class="fragment">We want to align our session being open with the duration of the request</p>

---

##### Commit

When we commit, we hit "save" on the database and data is transferred and written.

Too often and we transfer unnecessary data

Too little and we risk losing changes

---

## Exercise 2

Let's get started on the CRUD (**C**reate/**R**ead/**U**pdate/**D**elete) logic for our api

- Write a function that takes a session and the input data and insert it into the database
- Write a function that takes a session and listing_id and returns the listing from the database

---

## Solution 2

```python
def create_new_listing(session: Session,
                       username: str,
                       email: str,
                       address: str,
                       price: float
                       ) -> Listing:
    listing = Listing(username=username,
                      email=email,
                      address=address,
                      price=price)

    session.add(listing)
    return listing


def get_listing(session: Session, listing_id: int) -> Listing:
    stmt = sa.select(Listing).where(Listing.id == listing_id)
    result = session.execute(stmt).scalar()
    return result
```

---

## Update

To update a record, we can simply modify the attribute and add it back to the session

```python
with Session(engine) as session:
    listing = get_listing(session, 1)
    listing.username = "Anders Bogsnes"
    session.add(listing)
    session.commit()
```

---

## Delete

To delete is also simple

```python
with Session(engine) as session:
    listing = get_listing(session, 1)
    session.delete(listing)
    session.commit()
```

{{% /section %}}
