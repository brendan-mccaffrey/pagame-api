# Págame Financial Service

## About

This API was developed by the Pagame engineering team. Please contact brendan@pagame.org with any inquiries.

## Mission

### What

At Pagame, wour mission is to facilitate the digital transaction process and promote financial inclusion. 

### Why

Finances are a part of everyone's life. At Pagame, we believe that financial exclusion is neglection of the universal right to freedom. 

### How

This API is a culmination of secure and efficient data structures and function endpoints to process financial transactions instantaneously. The Pagame team prioritizes maintaining low barrier-to-entry into the network.


## Usage

All responses will be of the form:

```json
{
	"data": "Mixed type holding the content of the response",
	"message": "Description of what happened"
}
```

Subsequent response definitions will only detail

### List All Users

'GET /users'

**Response**

- '200 OK' on success

```json
[
	{
		"identifier": "123456789",
		"name": "John Smith",
		"type": "personal",
		"email": "john@gmail.com",
		"cell": "+591 7 123 4567"
	},
	{
		"identifier": "987654321",
		"name": "Maria Garcia",
		"type": "personal",
		"email": "marieg@gmail.com",
		"cell": "+591 7 765 4321"
	}
]
```

### Create New User

'POST /users'

**Arguments**

- '"identifier":string' a globally unique identifier
- '"name":string' government name of the user
- '"type":string' personal or business
- '"email":string' user email account
- '"cell":string' user cell phone number
- '"username":string' users Pagame username

**Response**

- '201 Created' on success

```json
[
	{
		"identifier": "987654321",
		"name": "Maria Garcia",
		"type": "personal",
		"email": "marieg@gmail.com",
		"cell": "+591 7 765 4321",
		"username": "mariag"
	}
]
```

### Lookup User Details

'GET /users/<identifier>'

**Arguments**

- '"identifier":string'

**Response**

- '404 User Not Found' if user does not exist
- '200 OK' on success

```json
[
	{
		"identifier": "987654321",
		"name": "Maria Garcia",
		"type": "personal",
		"email": "marieg@gmail.com",
		"cell": "+591 7 765 4321",
		"username": "mariag",
		"sources": 
		{
			"identifier": "A4Hruu78KY",
			"identifier": "B4Hruu78KI",
			"identifier": "X4Hruu78KI"
		},
		"transactions":
		{
			"identifier": "A4Hrheuuu78KY",
			"identifier": "B4H92u78KIiw3",
			"identifier": "X4Hrnf735b8KI"
		}
		
	}
]
```

### Delete User

'DELETE /users/<identifier>'

**Response**

- '404 User Not Found' if user does not exist
- '204 No Content' on success

### Update User

'PUT /users/<identifier>'

**Arguments**

- '"name":string' government name of the user 
- '"type":string' personal or business
- '"email":string' user email account
- '"cell":string' user cell phone number
- '"username":string' users Pagame username

Only necessary arguments (those to be updated) must be included - others may be left blank.

### Create New Transaction

'POST /users/<identifier>/transactions'

**Arguments**

- '"email":string' recipient's email account
- '"cell":string' recipient's cell phone number
- '"username":string' recipient's Pagame username

**Response**

- '404 User Not Found' if user does not exist
- '405 Recipient Unavailable' if recipient cannot be reached
- '201 Created' on success

```json
[
	{
		"identifier": "0XAD32hy",
		"init-timestamp": "1596709140",
		"initiator": "987654321",
		"init-geostamp": "PTR63jiu",
		"type": "send",
		"recipient": "123456789",
		"recip-geostamp": "J3fy8fn",
		"status": "pending",
		"recip-timestamp": "N/A"
	}
]
```

### Cancel Transaction

'DELETE /users/<identifier>/transactions/<identifier>'

**Response**

- '404 User Not Found' if user does not exist
- '406 Transaction Comitted' if the transaction cannot be undone
- '407 Transaction Not Found' if the transaction does not exist
- '201 No Content' on success

### List All User Transactions

'GET /users/<identifier>/transactions'

**Response**

- '404 User Not Found' if user does not exist
- '200 OK' on success

```json
[
	{
		"identifier": "0XAD32hy",
		"init-timestamp": "1596709140",
		"initiator": "987654321",
		"init-geostamp": "PTR63jiu",
		"type": "send",
		"recipient": "123456789",
		"recip-geostamp": "J3fy8fn",
		"status": "pending",
		"recip-timestamp": "N/A"
	},
	{
		"identifier": "0XArh2hy",
		"init-timestamp": "1596709141",
		"initiator": "987654321",
		"init-geostamp": "PTR63jiu",
		"type": "request",
		"recipient": "123456789",
		"recip-geostamp": "J3fy8fn",
		"status": "complete",
		"recip-timestamp": "1586347972"
	}
]
```

### Respond to Transaction

'PUT /users/<identifier>/transactions/<identifier>'

**Arguments**

- '"Accept":boolean' true to accept, false to decline

**Response**

- '404 User Not Found' if user does not exist
- '406 Transaction Comitted' if the transaction cannot be undone
- '407 Transaction Not Found' if the transaction does not exist
- '200 Success' on success

```json
[
	{
		"identifier": "0XAD32hy",
		"init-timestamp": "1596709140",
		"initiator": "987654321",
		"init-geostamp": "PTR63jiu",
		"type": "send",
		"recipient": "123456789",
		"recip-geostamp": "J3fy8fn",
		"status": "denied",
		"recip-timestamp": "N/A"
	}
]
```

### Create New Source

'POST /users/<identifier>/sources'

**Arguments**

- '"issuer":string' source provider (issuing bank/entity)
- '"type":string' type of account (checking, debit, etc)
- '"routing":string' source routing number
- '"account":string' source account number
- '"validation":string' unique password/key for account

**Response**

- '404 User Not Found' if user does not exist
- '402 Source Not Found' if the source cannot be found
- '407 Source Unsupported' if the source is not compatible
- '200 Success' on success

```json
[
	{
		"identifier": "0XAD32hy",
		"user": "123456789",
		"issuer": "Banco Nacional Bolivia",
		"type": "checking",
		"account": "123456789",
		"routing": "12345678765678",
		"status": "enabled",
		"balance": "713.00"
	}
]
```

### Update Source

'PUT /users/<identifier>/sources/<identifier>'

**Arguments**

- '"action":string' temporarily disable or enable

**Response**

- '404 User Not Found' if user does not exist
- '402 Source Not Found' if the source cannot be found
- '408 Action Unexecutable' if action cannot be executed (ex: enable invalid source)
- '200 Success' on success


```json
[
	{
		"identifier": "0XAD32hy",
		"user": "123456789",
		"issuer": "Banco Nacional Bolivia",
		"type": "checking",
		"account": "123456789",
		"routing": "12345678765678",
		"status": "disabled",
		"balance": "713.00"
	}
]
```

### List User Sources

'GET /users/<identifier>/sources'

**Response**

- '404 User Not Found' if user does not exist
- '200 OK' on success

```json
[
	{
		"identifier": "0XAD652hy",
		"user": "123456789",
		"issuer": "Banco Nacional Bolivia",
		"type": "savings",
		"account": "123456789",
		"routing": "12345678765678",
		"status": "disabled",
		"balance": "713.00"
	},
	{
		"identifier": "0XAD02hy",
		"user": "123456789",
		"issuer": "Tigo Money",
		"type": "checking",
		"account": "123456789",
		"routing": "12345678765678",
		"status": "enabled",
		"balance": "713.00"
	}
]
```



