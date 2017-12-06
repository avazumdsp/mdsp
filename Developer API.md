## Contents

* [Introduction](#introduction)
* [Authentication](#authentication)
* [Reporting API](#reporting-api)
* [Campaign API](#campaign-api)
* [Changelog](#changelog)

## Introduction

In order to grab information through the API, the client must first be granted access, using your client id and secret to get your reporting access token. Do not share your client id and secret with external parties you do not want having access to all your data.


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
|startdate|Yes|String|Timezone: UTC<br>Format: YYYY-MM-DD<br>The date range from startdate to the current date must be less than or equal to 180 days.|
|enddate|Yes|String|Timezone: UTC<br>Format: YYYY-MM-DD<br>Enddate must be larger than or equal to startdate, and the date range from the startdate to the end date must be less than or equal to 30 days.|
|id|No|String|ID of the campaign or creative.|
|groupby|No|String|Which variable will be grouped by, note that "groupby" filed only for campiagn and creative dimension reports, can be one of the following value:<br>Value -- Description<br>creative -- creative<br>geo -- country<br>city -- city<br>gender --  gender<br>carrier -- carrier<br>isp -- isp<br>device -- device<br>devicetype -- device type<br>browser -- mobile browser<br>os -- operation system<br>osv -- operation system version<br>connection -- connection type<br>inventory -- inventory<br>publisher -- publisher(seller)<br>site -- site<br>inventorytype -- inventory type|
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
||date|No|String|Timezone: UTC<br>Format: YYYY-MM-DD<br>Note that the field "date" only for campaign and creative dimension reports|
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
    "enddate":"2016-01-02"
}
```

Response for **user** dimension reports
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "totalcount": 2,
    "data": [
        {
        "impressions": "5575",
        "clicks": "11",
        "conversions": "0",
        "spend": "13.11"
        }
    ]
}
```

Request for **campaign** dimension reports
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "campaign",
    "startdate": "2016-01-01", 
    "enddate":"2016-01-02"
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
            "date":"2016-01-01",
            "campaign_id": "36168",
            "impressions": "5574",
            "clicks": "11",
            "conversions": "0",
            "spend": "13.69",
            "campaign_name": "test publisher"
        },
        {
            "date":"2016-01-01",
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
    "enddate":"2016-01-02"
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
            "date":"2016-01-01",
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
            "date":"2016-01-01",
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
|status|Yes|int|Campaign status<br>Value -- Status<br>1 -- Active<br>0 -- Inactive|
|bidtype|No|int|Campaign bid type<br>Value -- Bid Type<br>0 -- CPM<br>1 -- CPM optimized towards to an eCPC<br>2 -- CPM optimized towards to a CTR<br>3 -- CPM optimized towards to a CPA<br>4 -- CPC<br>11 -- CPA|

##### 1.3 Response Object

|Field ||Required |Type |Description |
|----------|----------|----------|---------|----------|
|code||Yes|String|Status code|
|msg||Yes|String|Description details|
|data[]||Yes|Array||
||id|Yes|String|Campaign id|
||name|Yes|String|Campaign name|
||status|Yes|int|Campaign status<br>Value--Status<br>1--Active<br>0--Inactive|
||bidtype|Yes|int|Campaign bid type<br>Value -- Bid Type<br>0 -- CPM<br>1 -- CPM optimized towards to an eCPC<br>2 -- CPM optimized towards to a CTR<br>3 -- CPM optimized towards to a CPA<br>4 -- CPC<br>11 -- CPA|
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
||status|No|int|Campaign status<br>Value--Status<br>1--Active<br>0--Inactive|
||bidprice|No|int|Campaign bid price<br>Value--Bid Type--Available Min Bid Price--Available Max Bid Price<br>0 -- CPM-- 30000 --20000000<br>1 -- CPM optimized towards to an eCPC -- 30000 --20000000<br>2 -- CPM optimized towards to a CTR -- 30000--20000000<br>3 -- CPM optimized towards to a CPA -- 30000 --20000000<br>4 -- CPC -- 3000--5000000<br>11 -- CPA -- 100000 --10000000|

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
## Targeting API

### 1 Include/Exclude Apps/Sites API

##### 1.1 Request Endpoint

|HTTP Method |Endpoint |
|----------|----------|
|Post |http://api.mdsp.avazutracking.net/targeting |

##### 1.2 Request Object

|Field ||Required |Type |Description |
|----------|----------|----------|----------|----------|
|access_token||Yes|string|The access token issued by Avazu authorization server.|
|command||Yes|string|Use the following value to include/exclude<br>apps/sites<br>"add"<br>"delete"|
|data[]||Array||
||campaign_id|Yes|int|id of campaign that want to make changes|
||op_type|Yes|int|operation type<br>Value--Status<br>1--include<br>0--exclude|
||inventory_id|Yes|int|inventory id of the apps/sites belong
||site_id|Yes|string|1 app/site id or an array of apps/sites' ids<br>11111<br>aaabbcc|

##### 1.3 Sample
Request for include/exclude apps/sites
```javascript
{
    "access_token": "372ea8dade94379ab44f59d80c137fdb2fd937f4",
    "command": "add",
    "data": [
        {
            "campaign_id": "12345",
            "op_type": 1,
            "inventory_id":1001
            "site_id": 114855
        },
        {
            "campaign_id": "23456",
            "op_type": 0,
            "inventory_id":1002
            "site_id": "d75125188e1f647a"
        },
    ]
}
```
Request for include/exclude apps/sites
```javascript
{
    "code": 0,
    "msg": "SUCCESS",
    "data": {"12345","1001","114855"}
}
```


## Changelog
|Version |Date |Update |
|----------|----------|----------|
|1.0.1|May 20th,2016|Completed the first version of Avazu mDSP Developer API Documentation.|
|1.0.2|May 4th,2017|Added campaign status API.|
|1.0.3|June 2nd,2017|Added campaign status API.|
|1.0.3.1|May 26th,2016|Modified Date to Date range for Reporting API<br>Modified campaign maximum bid price|
|1.0.4|Dec 7th,2017|Added Include/Exclude Apps/sites Campaign API|
