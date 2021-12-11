# Starlite

Starlite is a flexible and extensible ASGI API framework built on top of Starlette and Pydantic.

## Example: Controller Pattern

Starlite supports class API components called "Controllers". Controllers are meant to group logical subcomponents, for example - consider the following `UserController`:

```python3
from pydantic import BaseModel, UUID4
from starlite import Controller, delete, get, Partial, patch, post, put, Starlite


class User(BaseModel):
    first_name: str
    last_name: str
    id: UUID4


class UserController(Controller):
    path = "/users"

    @post()
    async def create(self, data: User) -> User:
        ...

    @get()
    async def get_users(self) -> list[User]:
        ...

    @patch()
    async def partial_update_user(self, data: Partial[User]) -> User:
        ...

    @put()
    async def bulk_update_users(self, data: list[User]) -> list[User]:
        ...

    @get(path="/{user_id}")
    async def get_user_by_id(self, user_id: UUID4) -> User:
        ...

    @delete(path="/{user_id}")
    async def delete_user_by_id(self, user_id: UUID4) -> User:
        ...


app = Starlite(route_handlers=[UserController])

```