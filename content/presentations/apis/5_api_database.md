---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Connecting FastAPI and the Database

---

Now we have the basic CRUD setup, we need to hook up our handler functions to use the database

---

## Dependency Injection

Remember, we want our connections to be open for the duration of the request and then close it.

FastAPI lets us `Depend` on a function to have run when we execute, so FastAPI can keep track of what has run

---

First we define a `get_session` function

```python
def get_session():
    with Session(engine) as session:
        yield session
```

---

Change the endpoint

```python
@api.get("/listing/{listing_id}", response_model=ListingOutSchema)
def get_listing(listing_id: int, db: Session = Depends(get_db)):
    return get_listing_by_id(session=db, listing_id=listing_id)
```

---

### Pydantic ORM mode

Pydantic expects to receive dictionaries, and we are passing it a `Listing` object.

Luckily, Pydantic objects can be put into `ORM mode` to allow passing objects, like a `Listing`

```python
class ListingOutSchema(BaseModel):
    username: str
    address: str
    price: float

    class Config:
        orm_mode = True
```

---

## Exercise 2

Turn the API into a full CRUD application for handling listings. Think about what HTTP methods to use for each one

- Write a route for creating a new listing
- Write a route for getting a listing
- Write a route for updating a listing
- Write a route for deleting a listing

Try to write a route that gets all listings

---

## Solution 2

```python
from typing import List

import sqlalchemy as sa
from fastapi import FastAPI, Depends, status
from pydantic import BaseModel, EmailStr
from sqlalchemy.orm import declarative_base, Session


Base = declarative_base()
engine = sa.create_engine("sqlite:///airbnb.db", future=True)

api = FastAPI()


class Listing(Base):
    __tablename__ = "listings"

    id = sa.Column(sa.Integer, primary_key=True)
    username = sa.Column(sa.String)
    email = sa.Column(sa.String)
    address = sa.Column(sa.String)
    price = sa.Column(sa.Float)


class ListingOutSchema(BaseModel):
    username: str
    address: str
    price: float

    class Config:
        orm_mode = True


class ListingInSchema(ListingOutSchema):
    email: EmailStr


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


def get_listing_by_id(session: Session, listing_id: int) -> Listing:
    return session.get(Listing, listing_id)


def delete_listing_by_id(session: Session, listing_id: int) -> None:
    listing = get_listing_by_id(session, listing_id)
    session.delete(listing)


def update_listing_by_id(session: Session,
                         listing_id: int,
                         data: ListingInSchema
                         ) -> Listing:
    listing = get_listing_by_id(session, listing_id)
    for key, val in data.dict().items():
        if hasattr(listing, key):
            setattr(listing, key, val)
    session.add(listing)
    return listing

def get_all_listings_from_db(session: Session) -> List[Listing]:
    stmt = sa.select(Listing)
    return session.execute(stmt).scalars().all()

def get_db():
    with Session(engine) as session:
        yield session


@api.get("/listing/{listing_id}",
         response_model=ListingOutSchema)
def get_listing(listing_id: int, db: Session = Depends(get_db)):
    return get_listing_by_id(session=db,
                             listing_id=listing_id)


@api.post("/listing", 
          response_model=ListingOutSchema,
          status_code=status.HTTP_201_CREATED)
def create_listing(listing: ListingInSchema,
                   db: Session = Depends(get_db)):
    new_listing = create_new_listing(session=db,
                                     **listing.dict())
    db.commit()
    return new_listing


@api.delete("/listing/{listing_id}",
            status_code=status.HTTP_204_NO_CONTENT)
def delete_listing(listing_id: int,
                   db: Session = Depends(get_db)):
    delete_listing_by_id(session=db, listing_id=listing_id)
    db.commit()


@api.patch("/listing/{listing_id}",
           response_model=ListingOutSchema)
def update_listing(listing_id: int,
                   listing: ListingInSchema,
                   db: Session =  Depends(get_db)):
    updated_listing = update_listing_by_id(session=db,
                                           listing_id=listing_id,
                                           data=listing)
    db.commit()
    return updated_listing

@api.get("/listings", 
         response_model=List[ListingOutSchema])
def get_all_listings(db: Session = Depends(get_db)):
    return get_all_listings_from_db(db)

```

{{% /section %}}