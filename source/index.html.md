---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - tokens
  - licenses
  - renewals
  - payments
  - officers

search: true
---

# Notes

REST API document for COMP9322 assignment 2.

## Use JSON or XML

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

Theoretically prefer PUT because it's idempotent. However, when updating by PUT, server will ignore some values because either server generates the values automatically or server doesn't trust the values given by the client.

See [Update a renewal notice](#renewals_update-a-renewal-notice) for example.

## Authentication

```http
POST /tokens HTTP/1.1
```

```http
GET /licenses HTTP/1.1
Authorization: token type=driver,id=1
```

```http
GET /licenses HTTP/1.1
Authorization: token b1c1d8
```

Every client that needs to access privileged APIs should have an access token.

We follow the HTTP design, use the token in `Authorization header`. A token can be simple key value pairs or a hash string.

Driver can get their access token by the renewal notice link in the email, which is like `http://clienthost.com/driver?renewalId=1&token=b1c1d8`

Officers can get their access token by [tokens API](#tokens_tokens) when logging in.

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
