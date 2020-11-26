---
weight: 90
outputs: ["Reveal"]
---

{{% section %}}

# Testing the API

---

Testing is one of the most important parts of building an API and FastAPI makes it simple

```python
# The FastAPI instance
from app import app
from fastapi.testclient import TestClient

client = TestClient(app)

def test_get_docs():
    response = client.get("/docs")
    assert response.status_code == 200
    assert "FastAPI" in response.text
```

---

The TestClient wraps the app in a `requests.Session`, so that we can use the `requests` API directly to talk to our app

---

## Structure for testing

When structuring the app, much like any software project, it's important to think about testing.

When building APIs, think about splitting up the functionality into separate components

- Routes for handling endpoints
- Services for handling business logic
- Models for handling database models
- Schemas for handling input-output validation

---

This structure allows us to test routes separately from business logic and allows for easy patching when testing routes

---

### Testing Routes

```python
class TestCanPostListing:
    @pytest.fixture(scope="class")
    def listing(self):
        return Listing(username="test",
                       email="test@test.com",
                       address="Test Adress",
                       price=700)

    @pytest.fixture(scope="class")
    def patched_service(self,
                        class_mocker: MockFixture,
                        listing: Listing) -> MagicMock:
        return class_mocker.patch("airbnb.routes.services.create_new_listing",
                                  return_value=listing)

    @pytest.fixture(scope="class")
    def response(self,
                 client: TestClient,
                 listing: Listing,
                 patched_service: MagicMock):
        return client.post("/listing",
                           json={"username": listing.username,
                                 "email": listing.email,
                                 "address": listing.address,
                                 "price": listing.price})

    def test_response_has_correct_status_code(self, response: Response):
        assert response.status_code == 201

    def test_response_has_correct_data(self,
                                       listing: Listing,
                                       response: Response):
        expected = {
            "username": listing.username,
            "address": listing.address,
            "price": listing.price
        }
        assert response.json() == expected
```

---

### Testing services

```python
import pytest
from sqlalchemy import select
from sqlalchemy.orm import Session

from airbnb.models import Listing
from airbnb.services import create_new_listing


class TestCanCreateListing:
    @pytest.fixture(scope="class")
    def listing(self) -> Listing:
        return Listing(username="test",
                       email="test@test.com",
                       address="Test Address",
                       price=700)

    @pytest.fixture()
    def new_listing(self, listing: Listing, db: Session):
        return create_new_listing(session=db,
                                  email=listing.email,
                                  username=listing.username,
                                  address=listing.address,
                                  price=listing.price)

    def test_can_create_new_listing(self,
                                    new_listing: Listing,
                                    listing: Listing):
        assert new_listing == listing

    def test_insert_listing_twice_creates_two_rows(self,
                                                   listing: Listing,
                                                   db: Session):
        for _ in range(2):
            create_new_listing(db,
                               username=listing.username,
                               email=listing.email,
                               address=listing.address,
                               price=listing.price
                               )

        results = db.execute(select(Listing)).scalars().all()
        assert len(results) == 2
```

{{% /section %}}