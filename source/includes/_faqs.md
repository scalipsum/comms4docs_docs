# FAQs

## FAQ Model

| Field      | Data Type | Required |
| ---------- | --------- | -------- |
| `_id`      | ID        | ✔️       |
| `question` | String    | ✔️       |
| `answer`   | String    | ✔️       |

**Validation** rules are only set by the field types and no other additional rules are set.

## Create a new question

> POST /faqs/

```JSON
{
    "question": "How frequently is the content updated?",
    "answer": "Weekly"
}
```

> Response

```JSON
{
    "message": "Success! New question created.",
    "question": {
        "_id": "5f696c4988e1d24316ba64af",
        "question": "How frequently is the content updated?",
        "answer": "Weekly",
        "__v": 0
    }
}
```

`Access: Admin`

`FAQs` or `Frequently Asked Questions` are a list of posts meant to help the users from the clients understand the platform better without the contacting the Admin.

To add `question`, a simple `POST` request containing the question and the answer will suffice.

For a successful request, valid strings must be passed.

## Get all the questions

> GET /faqs/

```JSON
{
    "message": "Success! Questions and Answers fetched!",
    "questions": [
        {},
        {},
        {}
    ]
}
```

`Access: User`

Using this query, a client will be able to show all the FAQs to the customers.

The results are sent as an array of objects.

## Get a specific question

> GET /faqs/:id

```JSON
{
    "message": "Success! Question fetched!",
    "question": {
        "_id": "5f2ed413cc3d4a7aec85c8d5",
        "question": "Test question 1?",
        "answer": "Test answer 1.",
        "__v": 0
    }
}
```

`Access: User`

This query can retrieve each question individually, as an object.

This can be useful if the client's functionality requires a personalised page for each question.

## Update a specific question

> PUT /faqs/:id

````JSON
{
    "question": "Updated question?"
}

> response

```JSON
{
    "message": "Success! Question updated",
    "question": {
        "_id": "5f2ed413cc3d4a7aec85c8d5",
        "question": "Updated question?",
        "answer": "Test answer 1.",
        "__v": 0
    }
}
````

`Access: Admin`

An FAQ can be updated using a `PUT` request.

The body sent can be either `question` or `answer` or **both** if updating both the question and the answer. Passing them separately, will update only that value.

## Delete a specific question

> DELETE /faqs/:id

```JSON
{
    "message": "Success! Question deleted!",
    "question": {
        "_id": "5f2ed413cc3d4a7aec85c8d5",
        "question": "Updated questoin?",
        "answer": "Test answer 1.",
        "__v": 0
    }
}

`Access: Admin`

Deleting a question will **permanently** remove the record of the question from the database.

A successful `DELETE` request implies removing both the question and the answer.

The **id** parameter in the URL must be a valid existing **faqs_id**, otherwise an explicit error will be thrown.
```
