# Phrases

## Phrase Model

| Field         | Data Type | Required |
| ------------- | --------- | -------- |
| `_id`         | ID        | ✔️       |
| `category_id` | ID        | ✔️       |
| `body`        | String    | ✔️       |

## Create a new phrase

> POST /phrases/

```JSON
{
    "category_id": "5f0ef4250856267a36ecab32",
    "body: "Phrase 1 from category 1"
}
```

> Response

```JSON
{
    "message": "Success! New phrase created",
    "phrase": {
        "_id": "5f6939c6acf8ec2c708e2510",
        "category_id": "5f327d86cc3d4a7aec85c939",
        "body": "Phrase 2 from category 1",
        "createdAt": "2020-09-21T23:39:50.033Z",
        "updatedAt": "2020-09-21T23:39:50.033Z",
        "__v": 0
    }
}
```

Although the structure is very simplistic, a phrase is the core piece of content inside the Comms4Docs API.

To create a phrase it is only necessary to pass a `body` inside the `POST` request and a **valid** `category_id` which will be associated to.

**Validation**

In the case of phrases, the validation is formed only from the types of fields passed.

`category_id` - **unique & valid** \
`body` - String

## Get all phrases

> GET /phrases/

```JSON
{
    "message": "Success! All Phrases fetched!",
    "phrases": [
        {},
        {},
        {}
    ]
}
```

`Access: User`

This query will retrieve all the phrases in the database in no particular order. This is a good method of keeping track of the number of the total phrases in the system, however it is **not recommended** to heavily use this query as it is memory heavy.

The results are sent as an array of objects.

<aside class="notice">
    This query retrieves all the phrases, not the categories they are part of.
</aside>

## Get all phrases by category

> GET /phrases/category/:id

```JSON
{
    "message": "Success! Phrases by category fetched!",
    "phrases": [
        {},
        {},
        {}
    ]
}
```

`Access: User`

This is the main way of consuming the phrases based on their category.

Once the category id is known, all the phrases in that category can be queried and used in a client.

The **id** parameter in the URL must be a valid existing **category_id**, otherwise an explicit error will be thrown.

<aside class="notice">
    The results are accessible by any type of user, free or premium. It is in the duty of the client to sort them based on the logged in user type.
</aside>

## Get a specific phrase

> GET /phrases/:id

```JSON
{
    "message": "Success! Single Phrase fetched!",
    "phrase": {
        "_id": "5f318a41cc3d4a7aec85c91c",
        "category_id": "5f31775ecc3d4a7aec85c8df",
        "body": "Do you drink at all?",
        "createdAt": "2020-08-10T17:56:17.900Z",
        "updatedAt": "2020-08-10T17:56:17.900Z",
        "__v": 0
    }
}
```

`Access: User`

Simple query to retrieve all the information about a single phrase. This might me useful in a scenario where a phrase has its own client-side display page.

A `category_id` is not needed, just the `_id` of the actual phrase.

The **id** parameter in the URL must be associated to valid existing **phrase**, otherwise an explicit error will be thrown.

<aside class="notice">
    The phrase queried might be part of a premium category and it is in the duty of the client to check the logged in user type.
</aside>

## Add a phrase to favorites

> PUT /users/:id

```JSON
{
    "add_fav_phrase": "5f32bc4fcc3d4a7aec85c94b"
}
```

> Response

```JSON
{
    "message": "Success! User updated",
    "user": {
    }
}
```

`Access: User`

Adding a Phrase to favorites is actually part of the updating a `User` process.

When a new user creates an account, a field called `fav_phrases` is automatically created and consists of an empty array.

The process of adding a Phrase to favorites is nothing else then just adding phrases objects to the said array.

<aside class="notice">
    Since the user is already logged in, all the favorite phrases are accessible through the check-session query. You can read more about this <a href="/#authenticate-a-user">here</a>.
</aside>

In order to have a 200 response, **both ids** must be valid and exiting user/phrase ids. Otherwise, an error will be sent back.

## Remove a phrase from favorites

> PUT /users/:id

```JSON
{
    "rm_fav_phrase": "5f32bc4fcc3d4a7aec85c94b"
}
```

> Response

```JSON
{
    "message": "Success! User updated",
    "user": {
    }
}
```

`Access: User`

Removing a phrase from the favorites is exact same process as adding to favorites, but in reverse.

A `PUT` request with the logged in user id as `parameter` is necessary to perform a phrase remove from favorites, which will update the specific user array of favorite phrases.

You can read a more in depth explanation [here](/#add-a-phrase-to-favorites)

## Update a specific phrase

> PUT /phrases/:id

```JSON
{
    "body": "Updated phrase body"
}
```

> Response

```JSON
{
    "message": "Success! Phrase updated",
    "phrase": {
        "_id": "5f6939c6acf8ec2c708e2510",
        "category_id": "5f327d86cc3d4a7aec85c939",
        "body": "Updated phrase content",
        "createdAt": "2020-09-21T23:39:50.033Z",
        "updatedAt": "2020-09-22T03:01:37.397Z",
        "__v": 0
    }
}
```

`Access: Admin`

This query updates the body of a phrase.

It is an action that can only be performed by admin users as it is main core piece of the content.

The **id** parameter in the URL must be associated to valid existing **phrase**, otherwise an explicit error will be thrown.

## Delete a specific phrase

> DELETE /phrases/:id

```json
{
	"message": "Success! Phrase deleted."
}
```

`Access: Admin`

Deleting a specific phrase means that the record associated to that **phrase_id** will be **forever** deleted from the database.

This also means that the phrase will be deleted from the category that it is a part from.

In order for a request to be successful, a **valid stored user id** must be added as a parameter.
