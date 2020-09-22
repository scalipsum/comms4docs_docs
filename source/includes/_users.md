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

**subscription enum**

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

**Validation**

### User

`name` - alphanumeric value \
`email` - **unique** alphanumeric with @ and . \
`password` - min length 6, at least one lower case and an uppercase
`usage_history` - integers \
`subscription` - enum value \
`grade` - alphanumerics and underscore \
`role` - enum value

The password will be hashed and salted, and is never stored in plain text.

### Fav Phrase

`category_id` - valid category id

## Authenticate a user

> POST /auth/login

```JSON
{
    "email": "admin_email@gmail.com",
    "password": "******"
}
```

> Response

```JSON
{
    "message": "Success! Credentials are correct.",
    "user": {
        "_id": "******",
        "secret": "**********"
    }
}
```

> POST /auth/verify-token

```JSON
{
    "_id": "******",
    "secret": "**********",
    "token": "086706"
}
```

> Response

```JSON
{
    "message": "Success! Logged in as admin.",
    "admin": {
    }
}
```

> GET /auth/check-session

```JSON
{
    "message": "Success! Session found!",
    "user": {
    }
}
```

> GET /auth/logout

```JSON
{
    "message": "Success! You've been logged out."
}
```

`Access: Everyone`

As mentioned in the [Authentication](/#authentication), there are 2 user types that can authenticate: **admin** and **customer**.

### Admin

If the account if of an **admin type**, the user must go through 2 levels of authentication: **Email & Password** and **6-Digit Phone Code**. The Token will not be created unless both requirements are passed.

Once the first level of encryption is passed, the **user id** and a **secret** are sent as a response, both being needed in the next level.

The second level of encryption is what will create and store the Token in the database. In order to pass 3 things are needed:

- User ID
- Secret
- Phone Code

The Phone Code is generated using an Authenticator app on Android or iOS by firstly generating a **QR Code** with the link passed on user registration and then scanning it in the app. The 6 digit code is refreshed once at 30 seconds.

### Customer

Customers are logged in from the clients using only their **Email** and **Password**. If both are correct, a session token is generated and stored in the database for **one year**. After that, a re-login is necessary.

### Session Check

To check if there is a session for the current user, a simple `POST` request to the server will suffice.

### Logout

Once a logout request is made, the server will search for the current session, will destroy it and also delete the record from the database.

## Get all users

> GET /users/

```JSON
{
    "message": "Success! All users fetched!",
    "users": [
        {},
        {},
        {}
    ]
}
```

`Access: Admin`

Query to get information about all the users stored in the database.

The results are sent as an array of objects.

## Get last week users

> GET /users/lastweek

```JSON
{
    "stats": [
        {
            "date": "2020-09-14T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-15T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-16T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-17T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-18T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-19T23:00:00.000Z",
            "users": 4
        },
        {
            "date": "2020-09-20T23:00:00.000Z",
            "users": 5
        }
    ]
}
```

`Access: Admin`

Shows the **day-to-day** total number of users registered between today and 7 days ago.

This can be useful for showing a growth graph in the client side.

An array of objects is sent back, each object containing a date and the total number of users from that date.

## Get a specific user

> GET /users/:id

```JSON
{
    "message": "Success! Single Phrase fetched!",
    "user": {
        "usage_history": 39997,
        "subscription": "free",
        "role": "customer",
        "_id": "******",
        "fav_phrases": [
            {
                "_id": "******",
                "category_id": "******",
                "body": "So, in a typical week how often do you go out to the pub would you say?",
                "createdAt": "2020-08-10T17:56:49.297Z",
                "updatedAt": "2020-08-30T09:48:17.724Z"
            }
        ],
        "name": "App user 1",
        "email": "appuser@gmail.com",
        "last_active": "2020-08-30T10:56:21.990Z",
        "password": "******",
        "grade": "5",
        "speciality": "Internal Medicine",
        "tts_accent": {
            "label": "English - United Kingdom",
            "value": "en-GB"
        },
        "secret": {
            "ascii": "******",
            "otpauth_url": "******"
        },
        "createdAt": "2020-08-05T11:53:12.213Z",
        "updatedAt": "2020-08-30T10:56:22.945Z",
        "__v": 495
    }
}
```

`Access: Admin`

Get all the stored information about a specific user.

<aside class="notice">
This will also retrieve all the user's favorite Phrases.
</aside>

In order for a request to be successful, a **valid stored user id** must be added as a parameter.

Each user is stored and sent back as an object type.

## Update a specific user

> PUT /users/:id

```JSON
{
    "name": "John Doe"
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

There are **2 ways** a user can be updated:

1. By being **logged in as himself** and updating its own information
2. Information is being updated **by an Admin**

Each field can be updated individually so it is only necessary to be passed a single field in order for a user to be updated.

<aside class="notice">
Fields sent in the updated body must comply with all the Validation rules set in the Model.
</aside>

In order for a request to be successful, a **valid stored user id** must be added as a parameter.

To add or remove a phrase from favorites, jump to [this chapter]().

## Delete a specific user

> DELETE /users/:id

```json
{
	"message": "Success! User deleted!"
}
```

`Access: User`

Deleting a specific user means that the record associated to that **user id** will be **forever** deleted from the database.

With this action, **all the saved favorite Phrases will be deleted**.

If the user is **currently logged in**, first, the session stored in the database is deleted and only then the whole user object.

In order for a request to be successful, a **valid stored user id** must be added as a parameter.

**Any logged in admin** is also empowered to delete all the information about any user.
