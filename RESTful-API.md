# RESTful API

## PUT vs. PATCH

### TL;DR

**PUT** is used to replace the entire entity.
**PATCH** is used to patch the entity.

----

When using PUT, it is assumed that you are sending the complete entity, and that complete entity *replaces* any existing entity at that URI.

```
{ "username": "skwee357", "email": "skwee357@domain.com" }
```

If you POST this document to /users, as you suggest, then you might get back an entity such as

```
## /users/1
{
    "username": "skwee357",
    "email": "skwee357@domain.com"
}
```

If you want to modify this entity later, you choose between PUT and PATCH. A PUT might look like this:

```
PUT /users/1
{
    "username": "skwee357",
    "email": "skwee357@gmail.com"       // new email address
}
```

You can accomplish the same using PATCH. That might look like this:

```
PATCH /users/1
{
    "email": "skwee357@gmail.com"       // new email address
}
```

### Using PUT wrong

```
GET /users/1
{
    "username": "skwee357",
    "email": "skwee357@domain.com"
}
PUT /users/1
{
    "email": "skwee357@gmail.com"       // new email address
}

GET /users/1
{
    "email": "skwee357@gmail.com"      // new email address... and nothing else!
}
```

reference: https://stackoverflow.com/a/34400076