# Payments

Payment is made by driver and is only created when driver takes the paying action.

The amount that a driver needs to pay is stored in [renewal notice](#renewals_renewals) `fee` field.

A payment cannot be updated or deleted, because it's kind of a record or logs. So PUT or DELETE method is not allowed.

## Attributes

Attributes | Description
---------- | -----------
id | int, database primary key
amount | numeric, the amount paid
paidDate | date time string, date of the required fee paid, in "DD/MM/YYYY hh:mm:ss" format
renewal | [renewal object](#renewals_renewals), not null, should only contain id and uri, referring to driver's license

## Get a payment

```http
GET /payments/1 HTTP/1.1
Authorization: token <ACCESS TOKEN>
Content-Type: application/json
```

> Returns

```json
{
  "id": 1,
  "uri": "/payments/1",
  "amount": 50,
  "paidDate": "27/09/2017 21:00:07",
  "renewal": {
    "id": 1,
    "uri", "/renewals/1"
  }
}
```

## Create a payment

```http
POST /payments HTTP/1.1
Authorization: token <ACCESS TOKEN>
Content-Type: application/json

{
  "amount": 50,
  "renewal": {
    "id": 1
  }
}
```

> Returns

```json
{
  "id": 1,
  "uri": "/payments/1",
  "amount": 50,
  "paidDate": "27/09/2017 21:00:07",
  "renewal": {
    "id": 1,
    "uri", "/renewals/1"
  }
}
```

A payment can only be created by drivers, after their renewal is confirmed or has been reviewed by an officer.

In this assignment, the payment creation will always be successful because we just simulate the payment process and it doesn't involve any real payment services.

