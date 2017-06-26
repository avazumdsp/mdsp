# A Self-serve Mobile Advertising Platform.

# Sign up for an account at [Avazu Mobile DSP](http://mdsp.avazutracking.net/home/).

# Introduction

In order to grab information through the API, the client must first be granted access, using your client id and secret to get your reporting access token. Do not share your client id and secret with external parties you do not want having access to all your data.


## Contents

* [Authentication](#authentication)
* [Reporting API](#reporting-API)
* [Campaign API](#campaign-API)

## Authentication

##### 1 Request Endpoint

|HTTP Method |Endpoint |
|----------|----------|
|Post |http://api.mdsp.avazutracking.net/auth/access_token |

##### 2 Request in JSON format
```javascript
{
    "client_id": "your client id",
    "client_secret": "your client secret",
    "grant_type": "client_credentials" //default value: client_credentials
}
```

##### 3 Response in JSON format
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "expires_in": 1451577601 //the token will expire in 15 minutes
}
```

## Reporting API

##### 1 Request Endpoint

|HTTP Method |Endpoint |
|----------|----------|
|Post |http://api.mdsp.avazutracking.net/reporting |

##### 2 Request Object

|Field |Required |Type |Description |
|----------|----------|----------|----------|
|access_token|Yes|String|The access token issued by Avazu authorization server.|
|command|Yes|String|Reports dimensions, can be one of the following value:<br>"user"<br>"campaign"<br>"creative"|
|startdate|Yes|String|Timezone: UTC<br>Format: YYYY-MM-DD<br>the date range from startdate to the current date must be less than or equal to 180 days|
|enddate|Yes|String|Timezone: UTC<br>Format: YYYY-MM-DD<br>enddate must be larger than or equal to startdate|
|id|No|String|ID of the campaign or creative.|
|groupby|No|String|Which variable will be grouped by, note that "groupby" filed only for campiagn and creative dimension reports, can be one of the following value:|
|page|No|int|Each page contains a maximum 100 items. Default value is 1. (Note that "page" field only for campaign and creative dimension reports)|

##### 3 Response Object

|Field ||Required |Type |Description |
|----------|----------|----------|---------|----------|
|code||Yes|String|Status code|
|msg||Yes|String|Description details|
|totalcount||No|int|Total amount of items|
|page||No|int|Page number currently displayed|
|pagemaxcount||No|int|Maximum items of each page|
|data[]||Yes|Array||
||campaign_id|No|String|Campaign id|
||creative_id|No|String|Creative id|
||impressions|Yes|String|Total amount of impressions|
||clicks|Yes|String|Total amount of clicks|
||conversions|Yes|String|Total amount of conversions|
||impressions|Yes|float|Total amount of money spent, in US Dollars|
||campaign_name|No|String|Campaign name|
||creative_name|No|String|Creative name|
||geo_name|No|String|Country name|
||city_name|No|String|City name|
||gender_name|No|String|Gender name|
||carriers_name|No|String|Carriers name|
||isp_name|No|String|ISP name|
||device_name|No|String|Device name|
||campaign_name|No|String|Campaign name|
||devicetype_name|No|String|Device type name|
||browser|No|String|Mobile browser name|
||os_name|No|String|OS name|
||osv|No|String|OS version name|
||connection_name|No|String|Connection type name|
||inventory_name|No|String|Inventory type name|
||publisher_name|No|String|Publisher(Seller) name|
||site_name|No|String|Site name|
||inventorytype_name|No|String|Inventory type name|

##### 4 Sample
Request for **user** dimension reports
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "user",
    "startdate": "2016-01-01",
    "enddate":"2016-01-07"
}
```

Response for **user** dimension reports
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "data": {
        "impressions": "5575",
        "clicks": "11",
        "conversions": "0",
        "spend": "13.11"
    }
}
```

Request for **campaign** dimension reports
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "campaign",
    "startdate": "2016-01-01", 
    "enddate":"2016-01-07"
}
```

Response for **campaign** dimension reports
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "totalcount": 2,
    "page": 1,
    "pagemaxcount": 100,
    "data": [
        {
            "campaign_id": "36168",
            "impressions": "5574",
            "clicks": "11",
            "conversions": "0",
            "spend": "13.69",
            "campaign_name": "test publisher"
        },
        {
            "campaign_id": "36213",
            "impressions": "0",
            "clicks": "0",
            "conversions": "0",
            "spend": "0.00",
            "campaign_name": "sma"
        }
    ]
}
```

Request for **creative** dimension reports (group by geo)
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "creative",
    "groupby": "geo",
    "startdate": "2016-01-01",
    "enddate":"2016-01-07"
}
```

Response for **creative** dimension reports (group by geo)
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "totalcount": 2,
    "page": 1,
    "pagemaxcount": 100,
    "data": [
        {
            "campaign_id": "36168",
            "creative_id": "281825",
            "impressions": "5574",
            "clicks": "0",
            "conversions": "0",
            "spend": "13.69",
            "creative_name": "14c350704",
            "campaign_name": "test globe publisher",
            "geo_name": "United States"
        },
        {
            "campaign_id": 36168,
            "creative_id": 281826,
            "impressions": "1110",
            "clicks": "11",
            "conversions": "0",
            "spend": "1.00",
            "creative_name": "14c350704541",
            "campaign_name": "test globe publisher",
            "geo_name": "China"
        }
    ]
}
```


## Campaign API

### 1 Get Campaign Status and Bid Price

##### 1.1 Request Endpoint

|HTTP Method |Endpoint |
|----------|----------|
|Post |http://api.mdsp.avazutracking.net/usercampaign |

##### 1.2 Request Object

|Field |Required |Type |Description |
|----------|----------|----------|----------|
|access_token|Yes|String|The access token issued by Avazu authorization server.|
|command|Yes|String|Use the following value to get all campaigns status:<br>"get"|
|page|Yes|int|Page number currently displayed|
|pagecount|Yes|int|Maximum items of each page|
|status|Yes|int|Campaign status|
|bidtype||int|Campaign bid type|

##### 1.3 Response Object

|Field ||Required |Type |Description |
|----------|----------|----------|---------|----------|
|code||Yes|String|Status code|
|msg||Yes|String|Description details|
|data[]||Yes|Array||
||id|Yes|String|Campaign id|
||name|Yes|String|Campaign name|
||status|Yes|int|Campaign status|
||bidtype|Yes|int|Campaign bid type|
||bidprice|Yes|int|Campaign bid price<br>e.g.: Bid Price = 1000000<br>Tip: $1 (USD) = 1000000|

##### 1.4 Sample

Request for get campaign status
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "get",
    "page": 1，
    "pagecount": 100,
    "status": 1
}
```

Response for get campaign status
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "data": [
        {
            "id": "12345",
            "name": "test1",
            "status": 1,
            "bidtype": 0,
            "bidprice": 1000000
        },
        {
            "id": "12346",
            "name": "test2",
            "status": 1,
            "bidtype": 4,
            "bidprice": 200000
        }
    ]
}
```

### 2 Modify Campaign Status and Bid Price

##### 2.1 Request Endpoint

|HTTP Method |Endpoint |
|----------|----------|
|Post |http://api.mdsp.avazutracking.net/usercampaign |

##### 2.2 Request Object

|Field ||Required |Type |Description |
|----------|----------|----------|---------|----------|
|access_token||Yes|String|The access token issued by Avazu authorization server.|
|command||Yes|String|Use the following value to modify campaign status:<br>"update"|
|data[]||Yes|Array||
||id|Yes|String|Campaign id|
||status||int|Campaign status|
||bidprice|Yes|int|Campaign bid price<br>|

##### 2.3 Response Object

|Field |Required |Type |Description |
|----------|----------|----------|---------|
|code|Yes|String|Status code|
|msg|Yes|String|Description details|
|data[]|Yes|JSON|Successful data|

##### 2.4 Sample

Request for modify campaign status
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "update",
    "data": [
        {
            "id": "12345",
            "status": 1,
            "bidprice": 1000000
        },
        {
            "id": "12346",
            "bidprice": 1000000
        },
        {
            "id": "12347",
            "status": 0
        }
    ]
}
```

Response for modify campaign status
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "data": {"12345":true,"12346":false,"12347":true}
}
```
