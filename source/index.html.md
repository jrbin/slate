---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - licenses
  - renewals
  - payments
  - officers

search: true
---

# Notes

REST API document for COMP9322 assignment 2.

<aside class="warning">
When and who will create payment is modified according to Professor's suggestion, please see <a href="#payments_payments">payments section</a> and <a href="#renewals_create-a-renewal-notice">Create a renewal notice</a>
</aside>

## Use JSON

I prefer using json for REST APIs because json is natively supported in javascript, making it really convenient.

## Resource Location URI

```json
{
  "id": 1,
  "uri": "http://myhost.com/licenses/1",
  "driverName": "Nima Nishad"
}
```

Every object representation should contain a uri of its location.

For simplicity, the examples of resources representation below don't contain `http://myhost.com` part. But bear in mind that we should have this part in real application.

## URI style

<aside class="notice">
Prefer shorter ones, like
</aside>

- `/licenses/1`
- `/renewals/1`
- `/payments/1`

<aside class="warning">
Not preferred
</aside>

- `/licenses/1/renewals/1`
- `/renewals/1/payments/1`
- `/licenses/1/renewals/1/payments/1`

<aside class="notice">
The following is the original discussion
</aside>

For resources with relationship (e.g one-to-one, one-to-many), we have two types of URI style.

For example, licenses and renewal notices is a one-to-many relationship. Every renewal belongs to a license. So for renewal, we can use either one of the following URIs

1. `/licenses/<licenseId>/renewals/<renewalId>`
2. `/renewals/renewalId`

The first one has clearer semantic information. But it needs provide extra information (i.e. the license ID) every time we use the renewal notice (either creating, updating).

In the second one, we can represent the relationship inside the resource object, like

> Here is a renewal object

```json
{
  "id": 1,
  "license": {
    "id": 1,
    "uri": "/licenses/1"
  }
}
```

But it's a little weird when creating a renewal notice using POST, because we need to provide the foreign key in the request body.

## Use PUT or PATCH

<aside class="notice">
Use PUT
</aside>

Theoretically prefer PUT because it's idempotent. However, when updating by PUT, server will ignore some values because either server generates the values automatically or server doesn't trust the values given by the client.

See [Update a renewal notice](#renewals_update-a-renewal-notice) for example.

## Authentication

> Drivers' authorisation

```http
GET /licenses HTTP/1.1
Authorization: token 03e8296d-566a-4405-bb03-bf4913794c90
```

> Officers' authorisation

```http
GET /licenses HTTP/1.1
Authorization: token 22ecc9e1-fa98-4ea8-a50b-8a074046b02c
```

<aside class="notice">
Use <code>03e8296d-566a-4405-bb03-bf4913794c90</code> for drivers
</aside>

<aside class="notice">
Use <code>22ecc9e1-fa98-4ea8-a50b-8a074046b02c</code> for officers
</aside>

As the professor said, the authorization and authentication will be much easier. All drivers will use the same token and all officers will use the another token. The two tokens are predefined and can be hardcoded.

## Errors

When a server error happens, the server should return an HTTP status other than `HTTP 200`. For simplicity, we are not trying to design a detailed error code system by ourselves on top of HTTP error codes. The following table list the HTTP status code the server should return in general cases.

HTTP status | Meaning
----------- | -------
400 | Bad Request -- Your request has unexpected parameters.
401 | Unauthorized -- Your access token is wrong.
404 | Not Found -- The requested resources could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid method.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

Note `50x` are runtime error, you should not return them yourself. Instead, you should let your program or server software (i.e. nginx) throw it.
