# Licenses

Represent a drivers license.

Licenses already exist in database. So there is no need to create a new driver's license (HTTP POST). We also don't have the function to remove a driver's license (HTTP DELETE).

## Attributes

Attributes | Description
---------- | -----------
id | int, database primary key
driverName |
address |
email |
licenseNumber |
licenseClass |
expiryDate | date string, date to expire, in "DD/MM/YYYY" format

## Get License

```http
GET /licenses/1 HTTP/1.1
Authorization: token <ACESS TOKEN>
```

```shell
curl "/licenses/1"
  -H "Authorization: token <ACESS TOKEN>"
```

> Returns

```json
{
  "id": 1,
  "driverName": "Nima Nishad",
  "address": "Unit 1/97-99 Birriga Road, Bellevue Hill, NSW",
  "licenseNumber": "123123123Q",
  "licenseClass": "Class C",
  "email": "youremail@email.com",
  "expiryDate": "10/10/2017"
}
```

Get driver's license information by database id.

Can only be done by license's owner or officers. It means drivers cannot search information of other drivers' licenses.

## Update License

```http
PUT /licenses/1 HTTP/1.1
Content-Type: application/json
Authorization: token <ACCESS TOKEN>

{
  "id": 1,
  "driverName": "Nima Nishad",
  "address": "Unit 1/97-99 Birriga Road, Bellevue Hill, NSW",
  "licenseNumber": "123123123Q",
  "licenseClass": "Class C",
  "email": "youremail@email.com",
  "expiryDate": "10/10/2022"
}
```

```shell
curl "/licenses/1"
  -X PUT
  -H "Content-Type: application/json"
  -H "Authorization: token <ACCESS TOKEN>"
  -d '{
        "id": 1,
        "driverName": "Nima Nishad",
        "address": "Unit 1/97-99 Birriga Road, Bellevue Hill, NSW",
        "licenseNumber": "123123123Q",
        "licenseClass": "Class C",
        "email": "youremail@email.com",
        "expiryDate": "10/10/2022"
      }'
```

> Returns an updated license, basically should be the same as above

```json
{
  "id": 1,
  "driverName": "Nima Nishad",
  "address": "Unit 1/97-99 Birriga Road, Bellevue Hill, NSW",
  "licenseNumber": "123123123Q",
  "licenseClass": "Class C",
  "email": "youremail@email.com",
  "expiryDate": "10/10/2022"
}
```

Update a driver's license.

Drivers can update their address in the beginning of the renewal. Officers can update the expiry date.
