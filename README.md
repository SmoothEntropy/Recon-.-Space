# Recon.Space API Documentation

## Overview

This document provides an overview of the Recon.Space API endpoints and their purposes.

### Endpoint Explanation

Recon.Space offers the capability to request data using endpoints, which are essentially URLs that look like this: `https://api.recon.space/myapi/<endpoint>/`. These endpoints provide responses in JSON format.

**Example:**

Request: `https://api.recon.space/myapi/orgnamegpspublic/`

Response:
```json
[{
    "id": 1539,
    "organisationname": "EMTech Global",
    "tags": [
        "IT-AI-Cyber-Data",
        "Manufacturer"
    ],
    "gps": "POINT(24.9347 60.1719)"
}, ...]
```

## Authenticated vs Public Endpoint

You can collect and fetch data from public endpoints without credentials by accessing the URL of the endpoint. Authenticated endpoints require the user to provide an access token obtained after a successful login.

### Example to Use Authenticated Endpoint:

1. Obtain the access token by logging in using `https://api.recon.space/myapi/connect/`.
2. Submit your credentials (login password).
3. Obtain your access token from the response.

```json
    "refresh": "eyJhbGciOi...JIUzI1NicCI6IkpXVCJ9.eyJ0b2tlbl90J1c...2VydHBvq88TMoyO...FB6IRtTbsK4",
    "access": "eyJhbG...ciOXVCJ9.eyJ0b2tlbl90eX...BlIjoiidXNlciJ9.gqhc...MvQwDbMg"
```


**Using the Access Token:**

- **Recon.Space App:** Log in to recon.space, use the advanced search page to search all data easily.
- **Curl:** Use the access token with curl.
- **Python3:** Use the access token with Python requests.

## List of Endpoints

All endpoint URLs start with `https://recon.space/myapi/`.

### Public Endpoints

#### Authentication Public Endpoints:

- `register/`: Allows a user to register an account by providing an email address and a password.
- `connect/`: Allows a user to connect to their account; an access token and a refresh token are provided.
- `refresh/`: Allows a user to get a new access token.

#### Data Public Endpoints:

- `records/`: Allows a user to get an insight into the database content.
- `orgnamepublic/`: Allows a user to get information about space organizations (50% of DB content).
- `orgnamegpspublic/`: Allows a user to get information about the localization of space organizations (33% of DB content).
- `weaponspublic/`: Allows a user to get information about space-related weapons (not all details).
- `tag/`: Allows a user to get all tags available for filtering purposes.

### Authenticated Endpoints

#### Authentication Authenticated Endpoints:

- `myaccount/`: Once logged in, you can check your account details.

#### Data Authenticated Endpoints:

- `orgname/`: Allows a user to get information about space organizations.
- `orgnamegps/`: Allows a user to get information about space organizations.
- `financial/`: Allows a user to get information about finance of a space organization.
- `satellite/`: Allows a user to get information about satellites of a space organization.
- `weapons/`: Allows a user to get information about space-related weapons.
- `domain/`: Allows a user to get information about domains owned by a space organization.
- `subdomain/`: Allows a user to get information about sub-domains used by a space organization.
- `ip/`: Allows a user to get information about IP addresses used by a space organization.
- `taglaws/`: Allows a user to get information of potential laws and guidelines to which a space organization is subject.

## Filters:

Certain endpoints can be used with filters. Rather than getting a list of all data and then searching within, you can use filters.

### orgnamepublic/:
- `orgname=spacex`
- `tags=Agency` or `tags=Agency,Manufacturer`

### orgnamegpspublic/:
- `orgname=spacex`
- `tags=Agency` or `tags=Agency,Manufacturer`

### orgname/:
- `orgname=spacex`
- `tags=Agency` or `tags=Agency,Manufacturer`
- `hassatellitenamed=Oneweb`
- `hassatelliteoperatedbycountry=Brazil`

### orgnamegps/:
- `orgname=spacex`
- `tags=Agency` or `tags=Agency,Manufacturer`

### satellite/:
- `satellitename=Tiang`
- `satellitecountryoperator=China`
- `satelliteorbit=GEO`
- `satellitelaunchvehicle=Falcon`


**Examples:**

- Looking for satellites in LEO: `https://api.recon.space/myapi/satellite/?satelliteorbit=LEO`
- Looking for satellites launched by Ariane and operated by China: `https://api.recon.space/myapi/satellite/?satellitelaunchvehicle=Ariane&satellitecountryoperator=China`
- Looking for the location of organizations that produced equipment and contain 'lock' in the name: `https://api.recon.space/myapi/orgname/?tags=manufacturer&orgname=lock`

**Usage with Curl:**

```bash
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT <youraccesstoken>" 'https://api.recon.space/myapi/orgname/?orgname=lock&tags=Manufacturer' | jq
```

## Use cases

- Looking for financial data of Lockheed Martin: 
  ```bash
  curl -X GET -H "Accept: application/json" -H "Authorization: JWT <youraccesstoken>" 'https://api.recon.space/myapi/orgname/3011/' | jq
  ```
- Looking for details about the internet domain where 3011 is the id of the domain:
  ```bash
  curl -X GET -H "Accept: application/json" -H "Authorization: JWT <youraccesstoken>" 'https://api.recon.space/myapi/domain/3011/' | jq
  ```
- Looking for the IP of a subdomain:
  ```bash
  curl -X GET -H "Accept: application/json" -H "Authorization: JWT <youraccesstoken>" 'https://api.recon.space/myapi/subdomain/57924/' | jq
  ```
- Looking for the GPS location of the www.lockheedmartin.com server:
  ```bash
  curl -X GET -H "Accept: application/json" -H "Authorization: JWT <youraccesstoken>" 'https://api.recon.space/myapi/ip/8348/' | jq
  ```

Example of Use case :

1 - Looking for orgname which contain lock in the name and that is taggued as Manufacturer. ( '| jq' at the end is optional)

```bash 
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space/myapi/orgname/?orgname=lock&tags=Manufacturer' |jq
```

```json
response:
{
  "id": 3011,
  "tags": [
    "Manufacturer",
    "Research"
  ],
  "domains": [
    {
      "id": 3011,
      "domain": "lockheedmartin.com"
    }
  ],
  "satellites": [],
  "organisationname": "Lockheed Martin",
  "countrycode": "US",
  "headquarterslocation": "Bethesda, Maryland, United States",
  "postalcode": "20817",
  "orgtype": "For Profit",
  "founded": "1912-01-01",
  "fundingtype": "",
  "totalfunding": 0,
  "employees": "10001+",
  "creator": "",
  "description": "Lockheed Martin is a global security and aerospace company specialized in advanced technology systems, products and services.",
  "websiteurl": "http://www.lockheedmartin.com",
  "twitterurl": "http://www.twitter.com/LockheedMartin",
  "facebookurl": "http://www.facebook.com/lockheedmartin",
  "linkedinurl": "http://www.linkedin.com/company/lockheed-martin",
  "contactemail": "community.relations@lmco.com",
  "contactphone": "(866)562-2363",
  "websitemontlyvisits": "1,529,369",
  "lastleadershiphiringdate": "",
  "patentsgranted": "4,295",
  "trademarksregistered": "502",
  "gps": "POINT(-77.13905284224023 39.03318067316889)",
  "spaceweapons": [
    22
  ],
  "financial": [
    891
  ]
}
```

2 - Looking for the financial data of lockheed

```bash
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space//myapi/orgname/3011/' |jq
```

3 - Looking for details about the internet domain where 3011 is the id of the domain (not the organzation id)

```bash
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space//myapi/domain/3011/' |jq
```
```json
response:
[...]
{
      "id": 8348,
      "ipaddress": "166.21.250.209"
    },
[...]
    {
      "id": 57924,
      "subdomain": "www.lockheedmartin.com",
      "port": "443"
    }
  ],
  "domain": "lockheedmartin.com"
}
[...]
```

3.1 - Looking for the ip of a of subdomain

```bash
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space//myapi/subdomain/57924/' |jq
```

```json
{
  "id": 55377,
  [...]
  "technologies": "F5 BigIP",
  "ipsubdom": "166.21.250.209",
  [...]
}
```

3.2 - Looking for the gps location of the www.lockheedmartin.com lockheed server.

```bash
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space//myapi/ip/8348/' |jq
```

```json
response:
 "id": 8348,
  "ipaddress": "166.21.250.209",
  [...]
  "iplongitude": "-97.822",
  "iplatitude": "37.751",
  "ipgeopoint": "POINT(-97.822 37.751)",
  [...]
}
```
