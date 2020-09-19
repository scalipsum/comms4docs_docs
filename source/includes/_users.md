# Users

## User Model

## Create a new user

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
