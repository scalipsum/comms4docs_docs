# Users

## User Model

| Field           | Data Type | Required |
| --------------- | --------- | -------- |
| `_id`           | ID        | ✔️       |
| `name`          | String    | ✔️       |
| `email`         | String    | ✔️       |
| `password`      | String    | ✔️       |
| `tts_accent`    | Object    |          |
| `usage_history` | Number    |          |
| `subscription`  | String    | ✔️       |
| `last_active`   | Date      |          |
| `grade`         | String    | ✔️       |
| `speciality`    | String    | ✔️       |
| `fav_phrases`   | Array     |          |
| `role`          | String    |          |
| `secret`        | Object    |          |

### fav_phrase Model

| Field         | Data Type | Required |
| ------------- | --------- | -------- |
| `category_id` | ID        | ✔️       |
| `body`        | String    | ✔️       |

### subscription enum

| Value     |
| --------- |
| `free`    |
| `premium` |

`DEFAULT: 'free'`

### role enum

| Value      |
| ---------- |
| `customer` |
| `admin`    |

`DEFAULT: 'customer'`

### usage history

`DEFAULT: '0' (seconds)`

## Create a new user

> POST /users/register

```JSON
{
    "name": "Gavin Belson",
    "email": "gavinbelson@gmail.com",
    "password": "********",
    "grade": "4",
    "speciality": "Internal medicine"
}
```

> Response

```JSON
{
    "message": "Success! New user created.",
    "user": {
        "usage_history": 0,
        "subscription": "free",
        "role": "customer",
        "_id": "5f67bc4097e4c843b00b9bf7",
        "name": "Gavin Belson",
        "email": "gavinbelson@gmail.com",
        "password": "$2a$10$ZrZAz0pCG9LmqgESWwQaRuSkrUopmS9Hj4mgO2l4Yy6Qf45ABIwFG",
        "tts_accent": {
            "label": "English - United Kingdom",
            "value": "en-GB"
        },
        "last_active": "2020-09-20T20:32:00.715Z",
        "grade": "4",
        "speciality": "Internal medicine",
        "secret": {
            "ascii": ".!4Isx#^qIN#l{%cQ8v,m<awDySrWywX",
            "otpauth_url": "otpauth://totp/Comms4Docs?secret=FYQTISLTPARV44KJJYRWY6ZFMNITQ5RMNU6GC52EPFJXEV3ZO5MA"
        },
        "fav_phrases": [],
        "createdAt": "2020-09-20T20:32:00.720Z",
        "updatedAt": "2020-09-20T20:32:00.720Z",
        "__v": 0
    }
}
```

`Access: Everyone`

User creation requires a `POST` request with a body passed that contains a minimum of all required fields in the model.

<aside class="notice">
A successful request will create a record in the database, but <b>not</b> a user session.
</aside>

A newly created user will automatically have assigned **british accent**, **0 usage history**, **customer role**, **free subscription**, **last active date**, **an ascii secret and a qr code url** that can be used within an `<img>` tag.

### Validation

### User

`name` - alphanumeric value \
`email` - unique alphanumeric with @ and . \
`password` - min length 6, at least one lower case and an uppercase
`usage_history` - integers \
`subscription` - enum value \
`grade` - alphanumerics and underscore \
`role` - enum value

### Fav Phrase

`category_id` - valid category id

## Authenticate a user

## Get all users

## Get last week users

## Get a specific user

## Update a specific user

## Delete a specific user

> The above command returns JSON structured like this:

```json
[
	{
		"id": 1,
		"name": "Fluffums",
		"breed": "calico",
		"fluffiness": 6,
		"cuteness": 7
	},
	{
		"id": 2,
		"name": "Max",
		"breed": "unknown",
		"fluffiness": 5,
		"cuteness": 10
	}
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET https://example.com/api/kittens`

### Query Parameters

| Parameter    | Default | Description                                                                      |
| ------------ | ------- | -------------------------------------------------------------------------------- |
| include_cats | false   | If set to true, the result will also include cats.                               |
| available    | true    | If set to false, the result will include kittens that have already been adopted. |

<aside class=success>
Remember — a happy kitten is an authenticated kitten!
</aside>

This endpoint retrieves a specific kitten.

<aside class=warning>
Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.
</aside>

### HTTP Request (with ID)

`GET https://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| ID        | The ID of the kitten to retrieve |
