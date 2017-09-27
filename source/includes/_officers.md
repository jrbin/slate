# Officers

Officers are people who send renewal notices to drivers and review cases manually.

An officer can take over a case, either accepts or rejects it with possible adjustment to the amount of the fee.

An officer needs to login to view all the information. For login process, see [Authentication](#notes_authentication) and [Tokens](#tokens_tokens).

We assume officers' username and passwords already exist, no need to create and modify them. So no POST, PUT and DELETE.

## Attributes

Attributes | Description
---------- | -----------
id | int, database primary key
username |
password |

## Get an officer

```http
GET /officers/1 HTTP/1.1
Authorization: token <ACCESS TOKEN>
```

> Returns

```json
{
  "id": 1,
  "username": "cooper",
  "password": "asldkjaslkdjalskdj"
}
```

## Get officer's renewal

```http
GET /officers/1/renewals HTTP/1.1
Authorization: token <ACCESS TOKEN>
```

> Returns

```json
[
  {
    "id": "1",
    "uri": "/renewals/1",
    "address": "Unit 1/97-99 Birriga Road, Bellevue Hill, NSW",
    "email": "youremail@email.com",
    "fee": 50,
    "status": 0,
    "issueDate": "27/09/2017 12:00:56",
    "lastModified": "27/09/2017 12:00:56",
    "license": {
      "id": 1,
      "uri": "/licenses/1"
    },
    "officer": {
      "id": 1,
      "uri": "officers/1"
    }
  }
]
```

Get all the renewal notices taken over by this officer. If current officer doesn't have any cases, return `[]`.
