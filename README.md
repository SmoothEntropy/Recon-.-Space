# Here is described how to used the endpoints and what purpose they serve.

### Endpoint explanation
Recon[.]Space offer the capability to request data using endpoint, basically that just an URL that looks like this: https://api.recon.space/myapi/<endpoint>/.
This will give provide a response in JSON format. 
Example:
Request : https://api.recon.space/myapi/orgnamegpspublic/
Response : [...] {
            "id": 1539,
            "organisationname": "EMTech Global",
            "tags": [
                "IT-AI-Cyber-Data",
                "Manufacturer"
            ],
            "gps": "POINT(24.9347 60.1719)"
        }, [...]

## Authenticated VS Public endpoint
You can collect and fetch data from the public endpoint without credential just by accessing the url of the endpoint.
Authenticated endpoints need the user to provide an access token that is obtained after a sucessful login.

### Example to use authenticated endpoint:
1- get the access token by log in using https://api.recon.space/myapi/connect/
2 - submit you credentials (login password)
3 - get your access token that should appear in the response like this:
{
    "refresh": "eyJhbGciOi...JIUzI1NicCI6IkpXVCJ9.eyJ0b2tlbl90J1c...2VydHlwZSI6InVzZXIifQ.Bu2wzzDCQBvq88TMoyO...FB6IRtTbsK4",
    "access": "eyJhbG...ciOXVCJ9.eyJ0b2tlbl90eX...BlIjoiYMTUwODk4XNlcl9pZCI6NjEsInVzZXJ0eXBlIjoidXNlciJ9.gqhc...MvQwDbMg"
}

4.1 use your access token using recon.space app:
Go to recon.space, log in, use the advanced search page to search all data easily. 

4.2 use your access token using curl:
curl -H "Authorization: JWT eyJhbG...ciOXVCJ9.eyJ0b2tlbl90eX...BlIjoiYMTUwODk4XNlcl9pZCI6NjEsInVzZXJ0eXBlIjoidXNlciJ9.gqhc...MvQwDbMg" https://api.recon.space/myapi/orgname/789/ |jq
info: |jq is to chain command with jq tool that you can install using apt install jq. It makes json output beautifuler :)

4.3 use you access token using python3:
import requests

auth_token='eyJhbG...ciOXVCJ9.eyJ0b2tlbl90eX...BlIjoiYMTUwODk4XNlcl9pZCI6NjEsInVzZXJ0eXBlIjoidXNlciJ9.gqhc...MvQwDbMg'
headers = {'Authorization': f'JWT {auth_token}'}

url = 'https://api.recon.space/myapi/orgname/789/'
response = requests.post(url,json=data, headers=headers)
print(response)
print(response.json())

List of endpoints
All endpoint urls start by https://recon.space/myapi/

## Public endpoint:
###Authentication public endpoints:
register/ using POST allows a user to register an account by providing an email address and a password.
connect/ using POST allows a user to connect to its account, an access token and a refresh token is then provided.
refresh/ using POST allows a user to get a new access token.

### Data public endpoints
records/ using GET allows a user to get an insight of the database content. 
orgnamepublic/ GET allows a user to get information about space organization. (50% of DB content)
orgnamegpspublic/ GET allows a user to get information about localisation of space organization. (33% of DB content)
weaponspublic/ GET allows a user to get information about space related weapons. (not all details)
tag/ GET allows a user to get all tags available for filtering purpose.

## Authenticated endpoints:
### Authentication Authenticated endpoints
myaccount/ once logged in you can check your account details.

### Data Authenticated endpoints
orgname/ GET allows a user to get information about space organization.
orgnamegps/ GET allows a user to get information about space organization.
financial/ GET allows a user to get information about finance of a space organization.
satellite/ GET allows a user to get information about satellite of a space organization.
weapons/ GET allows a user to get information about space related weapons.
domain/ GET allows a user to get information about domains owned by a space organization.
subdomain/ GET allows a user to get information about sub-domains used by a space organization.
ip/ GET allows a user to get information about information about ip addresses used by a space organization.
taglaws/ GET allows a user to get information of potential laws and guidelines to which a space organziation is subject to.


Filters:
Certain endpoints can be used with filters. Rather than getting a list of all data and then searching within, you can use filters.

orgnamepublic/:
orgname=spacex
tags=Agency or tags=Agency,Manufacturer

orgnamegpspublic/:
orgname=spacex
tags=Agency or tags=Agency,Manufacturer

orgname/:
orgname=spacex
tags=Agency or tags=Agency,Manufacturer
hassatellitenamed=Oneweb
hassatelliteoperatedbycountry=Brazil

orgnamegps/:
orgname=spacex
tags=Agency or tags=Agency,Manufacturer

satellite/:
satellitename=Tiang
satellitecountryoperator=China
satelliteorbit=GEO
satellitelaunchvehicle=Falcon

Examples:
Looking for satellite in LEO
https://api.recon.space/myapi/satellite/?satelliteorbit=LEO

Looking for satellite launched by Ariane and the satellite is operated by China
https://api.recon.space/myapi/satellite/?satellitelaunchvehicle=Ariane&satellitecountryoperator=China

Looking for location of orgnames that produced equipement and where 'lock' is contained in the name of this organization
https://api.recon.space/myapi/orgname/?tags=manufacturer&orgname=lock

Use case with curl:
1 - Looking for orgname which contain lock in the name and that is taggued as Manufacturer. ( '| jq' at the end is optional)

Command: curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'https://api.recon.space/myapi/orgname/?orgname=lock&tags=Manufacturer' |jq
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

2 - Looking for the financial data of lockheed
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'http://127.0.0.1:8000/myapi/orgname/3011/' |jq

3 - Looking for details about the internet domain where 3011 is the id of the domain (not the organzation id)
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'http://127.0.0.1:8000/myapi/domain/3011/' |jq
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

3.1 - Looking for the ip of a of subdomain
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'http://127.0.0.1:8000/myapi/subdomain/57924/' |jq
{
  "id": 55377,
  [...]
  "technologies": "F5 BigIP",
  "ipsubdom": "166.21.250.209",
  [...]
}
3.2 - Looking for the gps location of the www.lockheedmartin.com lockheed server.
curl -X GET  -H "Accept: application/json" -H "Authorization: JWT  <youraccesstoken>" 'http://127.0.0.1:8000/myapi/ip/8348/' |jq
response:
 "id": 8348,
  "ipaddress": "166.21.250.209",
  [...]
  "iplongitude": "-97.822",
  "iplatitude": "37.751",
  "ipgeopoint": "POINT(-97.822 37.751)",
  [...]
}