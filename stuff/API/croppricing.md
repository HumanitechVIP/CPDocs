---
layout: default
title: Crop Price Listings
parent: API
nav_order: 1
---
By Vincent Huynh
# Crop Price Listings

## Create Crop Listing
Create a crop listing by pushing a **POST** request:

```
POST http://croppricingapi.herokuapp.com/api/cropprices
```
### With Content
```
headers: {
    'Content-Type': 'application/json'
}
...
body: {
    {
        "code": "YYYY-MM-DD-crop",
        "unit": "Unit",
        "weight": 00,
        "variety": "variety",
        "market": "Market",
        "price": 0000
    }
}
```
### Returns
```
{
    "_id": "5fac27e331e9940017aa16c8",
    "code": "YYYY-MM-DD-crop",
    "unit": "Unit",
    "weight": "00",
    "price": 0000,
    "variety": "variety",
    "market": "Market",
    "date": "YYYY-MM-DDT(sometimestuff)",
    "__v": 0
}
```
### Errors
All errors from the server should be returned in a JSON format, contained in an array called "errors". For example, if you forgot to add the **weight** and **price** for a listing, you will get the following:

```
{
    "errors": [
        {
            "msg": "Weight required",
            "param": "weight",
            "location": "body"
        },
        {
            "msg": "Price required",
            "param": "price",
            "location": "body"
        }
    ]
}
```
This will allow us to loop through this array and see what errors we encountered.


### For example,

```
POST croppricingapi.herokuapp.com/api/cropprices
```
```
headers: {
    'Content-Type': 'application/json'
}
...
body: {
    {
        "code": "2020-10-15-deleteme3",
        "unit": "Bag",
        "weight": 90,
        "variety": "cereals",
        "market": "Nairobi",
        "price": 3000
    }
}
```
### Returns
```
{
    "_id": "5fac27e331e9940017aa16c8",
    "code": "2020-10-15-deleteme4",
    "unit": "Bag",
    "weight": "90",
    "price": 3000,
    "variety": "cereals",
    "market": "Nairobi",
    "date": "2020-11-11T18:05:23.841Z",
    "__v": 0
}
```
## Get a Crop Listing
```
GET croppricingapi.herokuapp.com/api/cropprices/listing/YYYY-MM-DD-crop
```
This get assumes you already know the listing based on the "code."
### Returns
```
{
    "_id": "5fac27e331e9940017aa16c8",
    "code": "2020-10-15-deleteme4",
    "unit": "Bag",
    "weight": "90",
    "price": 3000,
    "variety": "cereals",
    "market": "Nairobi",
    "date": "2020-11-11T18:05:23.841Z",
    "__v": 0
}
```
### If a Listing is Not Found:
```
{
    "msg": "Listing not found"
}
```

## Delete a Listing
To delete a listing, send a DELETE request to 
```
croppricingapi.herokuapp.com/api/cropprices/listing/YYYY-MM-DD-crop
```
### For example,
```
croppricingapi.herokuapp.com/api/cropprices/listing/2020-10-15-deleteme4
```
Returns the following after a successful removal.
```
{
    "msg": "Listing Removed"
}
```
In case there is no such listing (... if we send it again, for example), the following is returned:
```
{
    "msg": "Price Listing Not Found"
}
```


