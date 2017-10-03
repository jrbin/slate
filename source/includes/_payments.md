# Payments

<aside class="notice">
Payment design is modified.
</aside>

Payment is closely related to renewal notices. When a renewal notice is created, a payment resource is also created. So no explicit creation (POST) of a payment is allowed.

## Attributes

Attributes | Description
---------- | -----------
id | int, database primary key
amount | numeric, the amount paid
paidDate | date time string, date of the required fee paid, in "DD/MM/YYYY hh:mm:ss" format, is NULL when payment is not made
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
  "paidDate": null,
  "renewal": {
    "id": 1,
    "uri", "/renewals/1"
  }
}
```

## Update a payment

```http
PUT /payments/1 HTTP/1.1
Authorization: token <ACCESS TOKEN>
Content-Type: application/json

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

When a payment is actually paid by the driver. This operation will update the `paidDate` field to current time.
