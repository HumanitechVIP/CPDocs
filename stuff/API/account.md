---
layout: default
title: Account
parent: API
nav_order: 2
---
By Vincent Huynh
# Account

This portion of the API is meant for later use. Especially with the front-end. We hope to have an account system to track user's crop listing feedback. 

## Register a User
```
POST croppricingapi.herokuapp.com/api/users
```
```
headers: {
    'Content-Type': 'application/json'
}
body: {
    "name": "FirstName LastName",
	"email": "useremail@email.com",
	"password": "supersecretpassword"
}
```

### For example,
```
{
	"name": "HumanitechVIP Dummy",
	"email": "humanitechapp2@gmail.com",
	"password": "supersecretpassword"
}
```
Returns a token, which will be used for user validation. If the user's email as already been registered , the following will be returned:
```
{
    "errors": [
        {
            "msg": "User already exists"
        }
    ]
}
```
Once again, the errors are listed in the "errors" array. If the user puts too short of a password, the following is returned:
```
{
    "errors": [
        {
            "value": "s",
            "msg": "Please enter a password with 6 or more characters",
            "param": "password",
            "location": "body"
        }
    ]
}
```
We should probably do simple password length checks in the frontend so we don't needlessly push things to the API.

## Login User
To login the user, we can send 
```
POST croppricingapi.herokuapp.com/api/auth
```
with the content
```
headers: {
    'Content-Type': 'application/json'
}
body: {
    "email": "user@email.com",
	"password": "password"
}
```
### For example,
```
{
	"email": "humanitechapp@gmail.com",
	"password": "password"
}
```
returns a token, like so:
```
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjp7ImlkIjoiNWY2ZDA4YTFlMDY2NzEwMDE3NTczOWU4In0sImlhdCI6MTYwNTExOTUzOCwiZXhwIjoxNjA1MTIzMTM4fQ.C9kFKtgKpT-RLAwXM4za6zd9cEA_Xbkmp54WxLnwkH8"
}
```
If the user credentials are incorrect, the following is returned:
```
{
    "errors": [
        {
            "msg": "Credentials Invalid"
        }
    ]
}
```
Note that the error message for **both** the username **and** password are the exact same: this is intentional.

## Get User from Token
Once we have a Token from logging in, we can get the user's information.
```
GET croppricingapi.herokuapp.com/api/auth
```
```
headers: {
    'x-auth-token': '<THAT_LONG_TOKEN_FROM_LOGIN_OR_REGISTER>'
}
```
If we use the token generated before in the login, we should get the following user information returned:
```
{
    "level": 3,
    "_id": "5f6d08a1e0667100175739e8",
    "name": "HumanitechVIP",
    "email": "humanitechapp@gmail.com",
    "date": "2020-09-24T20:59:13.051Z",
    "__v": 0
}
```
Note that pretty much all user information is returned except for the password. We do not want to return the password back. No.

## Updating Password (Requires Token)
Once the user is logged in, they can update their password if needed.
```
POST croppricingapi.herokuapp.com/api/users/updatepassword
```
```
headers: {
    'x-auth-token': '<THAT_LONG_TOKEN_FROM_LOGIN_OR_REGISTER>'
}
body: {
    'password': 'newpassword'
}
```
If the token is valid, the API will return the user's information (without the password).
```
{
    "level": 3,
    "_id": "5f6d08a1e0667100175739e8",
    "name": "HumanitechVIP",
    "email": "humanitechapp@gmail.com",
    "date": "2020-09-24T20:59:13.051Z",
    "__v": 0
}
```
If the token is invalid, the API returns
```
{
    "msg": "INVALID TOKEN"
}
```