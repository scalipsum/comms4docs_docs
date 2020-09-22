# Categories

## Category Model

| Field                | Data Type | Required |
| -------------------- | --------- | -------- |
| `_id`                | ID        | ✔️       |
| `title`              | String    | ✔️       |
| `subscription`       | String    | ✔️       |
| `conversation_image` | String    |          |
| `conversation_audio` | String    |          |
| `conversation_body`  | String    |          |

**title** must be unique.

### subscription enum

| Value     |
| --------- |
| `free`    |
| `premium` |

`DEFAULT: 'free'`

## Create a new category

> POST /categories/

```JSON
formdata

name: 'Test Category'
subscription: 'Premium'
conversation_body: 'Conversation text for this category'
conversation_image: hello.jpg
conversation_audio: test.mp3
```

> Response

```JSON
{
    "message": "Success! New Category Created",
    "category": {
        "subscription": "premium",
        "_id": "5f692204acf8ec2c708e250f",
        "title": "Test Category",
        "conversation_body": "Conversation text for this category",
        "createdAt": "2020-09-21T21:58:28.692Z",
        "updatedAt": "2020-09-21T21:58:28.692Z",
        "__v": 0
    }
}
```

`Access: Admin`

Categories can be created using a `POST` request with a body that contains the required fields. They are the main way of filtering the newly created phrases.

Since there are 2 types of customers **free** & **premium**, the categories can be accessed by either one of them depending on the preference of the Admin. `Free` means being accessed by everyone and `premium` can only be accessed by the paid users.

If a category is `premium` all the phrases associated with it are by definition premium as well.

Each category has an `optional` **conversation** associated to it. A conversation is formed of an image, and audio file and written transcript. Therefore, `conversation_audio` & `conversation_body` are fields that accept a file.

**Validation**

### Category

`title` - **unique** \
`subscription` - enum value \
`conversation_audio` - Audio formats: **mpeg**, **ogg**, **wav**, **x-metroska**, **mp4**, max file size: **50 mb** \
`conversation_image` - Image formats: **jpg**, **gif**, **png**, max file size: **5 mb**

All the files are store in `/upload` using a naming convention: `conversation_audio-randomID` or `conversation_image-randomID`

## Get all categories

> GET /categories/

```JSON
{
    "message": "Success! Categories fetched!",
    "categories": [
        {},
        {},
        {}
    ]
}
```

`Access: User`

Query to retrieve all the categories stored in the database.

The results are sent as an array of objects.

<aside class="notice">
    This query does not retrieve the phrases associated with the categories.
</aside>

## Get a specific category

> GET /categories/:id

```JSON
{
    "message": "Success! Category fetched!",
    "category": {
        "subscription": "premium",
        "_id": "5f692204acf8ec2c708e250f",
        "title": "Test Category",
        "conversation_body": "Conversation text for this category",
        "createdAt": "2020-09-21T21:58:28.692Z",
        "updatedAt": "2020-09-21T21:58:28.692Z",
        "__v": 0
    }
}
```

`Access: User`

Query to retrieve a specific category from the database.

The result is sent as an object.

The **id** parameter in the URL must be a valid existing **id**, otherwise an explicit error will be thrown.

<aside class="notice">
    This query does not retrieve the phrases associated with the category.
</aside>

## Update a specific category

> PUT /categories/:id

```JSON
formdata

subscription: 'free'
```

> Response

```JSON
{
    "message": "Success! Category updated",
    "category": {
        "subscription": "free",
        "_id": "5f692204acf8ec2c708e250f",
        "title": "Test Category",
        "conversation_body": "Conversation text for this category",
        "createdAt": "2020-09-21T21:58:28.692Z",
        "updatedAt": "2020-09-21T22:17:37.414Z",
        "__v": 0
    }
}
```

`Access: Admin`

Using a `PUT` request each category can be updated. All the fields are checked individually, and there is no need to pass an entire category inside of the body, just the updated fields.

The **id** parameter in the URL must be a valid existing **id**, otherwise an explicit error will be sent back.

In the case where a file is updated by an Admin, (i.e. conversation_image, conversation_audio), the system first searches for that specific file in the `/uploads` folder, deletes the record, adds the new file with the new name and updates the database with the new path. This way the old files won't occupy unnecessary server space.

All updated fields sent in the body must comply with the Validation rules set in the Model / Controller.

**Validation**

`title` - **unique** \
`subscription` - enum value \
`conversation_audio` - Audio formats: **mpeg**, **ogg**, **wav**, **x-metroska**, **mp4**, max file size: **50 mb** \
`conversation_image` - Image formats: **jpg**, **gif**, **png**, max file size: **5 mb**

## Delete a specific category

> DELETE /category/:id

```json
{
	"message": "Success! Category deleted & all the corresponding phrases."
}
```

`Access: Admin`

Deleting a specific category means that the record associated to that **category id** will be **forever** deleted from the database.

With this action, **all the phrases** associated with that category are deleted.

Also, **all the files in the `/uploads`** associated with that category are deleted.

In order for a request to be successful, a **valid stored user id** must be added as a parameter.
