# Tokens

Used in authentication and authorisation.

Everytime the client application calls the REST api, it should include the token in the request header if the api needs authorisation.

Token is kind of different from other resources because it's not a database entity and doesn't carry any meaningful fields itself.

## Attribute

Attributes | Description
---------- | -----------
id | the only thing in a token, can be a int or other unique identifiers, represents a session and stores some user information

## Create Token

```http
POST /tokens HTTP/1.1
Content-Type: application/json

{
  "username": "cooper",
  "password": "mypass123"
}
```

```shell
curl /tokens
  -X POST
  -H "Content-Type: application/xml"
  -d "<token>
        <username>cooper</username>
        <password>mypass123</password>
      </token>"
```

> Returns

```json
{
  "id": "74215be5-2780-457e-81f9-775add78c4a5"
}
```

Create a token by username and password. Mainly used in officer login.

Drivers don't need to use it because they are not required to login. But drivers will be provided with a token in their renewal notice so that they can access the renewal service.

## Delete Token

```http
DELETE /tokens/74215be5-2780-457e-81f9-775add78c4a5 HTTP/1.1
```

```shell
curl /tokens/74215be5-2780-457e-81f9-775add78c4a5
  -X DELETE
```

> Returns

```json
{
  "id": "74215be5-2780-457e-81f9-775add78c4a5"
}
```

Used in officer log out and when driver wants to archive their renewal notice.
