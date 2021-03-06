# Introduction

> Following the instructions from the repo, you can access the local API at:

```
localhost:5000
```

Welcome to the Comms4Docs API! This is a Node.js based API used as a backend system for the Comms4Docs Mobile App as well as for the Comms4Docs [Admin Panel](https://comms4docs.co.uk).

You can find the production API hosted at [https://api.comms4docs.co.uk](https://api.comms4docs.co.uk) and if you wish to run it locally, the source code can be found in this [repository](https://github.com/scalipsum/comms4docs_api) (permission to access required)

# Authentication

> Example of a successfully set auth cookie:

```json
{
    _id: "****",
    expires: 2020-09-20T09:59:59.482+00:00,
    session: "{"cookie":{"originalMaxAge":86400000,"expires":"2020-09-20T09:54:00.592Z","httpOnly":true,"path":"/","sameSite":false},"passport":{"user":"****"}}
}


```

Comms4Docs uses 2 types of authentication:

- **Email** and **Password** for the **app users**
- **2 Factor Authentication** for the admin panel (**Email**, **Password**, **6-digit phone code**)

Both of the auth systems are based on [Passport.js](http://www.passportjs.org/) in conjunction with the [local strategy](http://www.passportjs.org/packages/passport-local/).

Each Session is maintained via an unique Cookie Token generated by Passport which is based on serializing and deserializing the according user. In order to maintain multiple sessions in tandem, they are stored securely in the database, **not** in the Local Storage.

`Each auth cookie lifetime is set for a year.`

The **2 Factor Authentication** is made possible using 3 levels of encryption:

- **QR Code Token** (generated when a user creates an account)
- **an ascii token** (generated once the admin user confirms the email and password)
- **a 6-digit phone code** (generated by scanning the qr code)

<aside class=notice>
The 6-digit phone code is generated within an Authenticator app and is renewed once at 30 seconds.
</aside>
