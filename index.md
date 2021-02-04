Our API allows you to interact with us using software. The API is RESTful and uses JSON to transport information. Use cases include the automatic creation of entries.

This API documentation will get you started with your first requests with our API. All requests require an active API Token, which you create yourself in our CMS.

## Endpoints

All endpoints use `https://api.xs2radio.com` as the base URL.

| | | |
|-|-|-|
`/v1/entries.json`  | POST  | [Create an entry](#create-entry)
`/v1/entries/123.json`  | GET  | [Read an entry](#read-entry)
`/v1/entries/213.json`  | PATCH  | [Update an entry](#update-entry)
`/v1/entries/123.json`  | DELETE  | [Delete an entry](#delete-entry)

## Response codes

Code                |Description
--------------------|---
`200 OK`            | The action was successful
`201 Created`       | The resource was created
`400 Bad Request`   | Missing or invalid fields
`401 Authorization Required`           | Missing or invalid API token
`500 Internal Server Error`              | A problem occurred on our side; please contact us to get it resolved

## Throttling

Usage is unlimited within the subscription and permissions of the account you are using. To prevent fraud and abuse, requests to the API are throttled. You can request the API 150 times each 5 minutes. If you go over, you will see an error response until enough time has elapsed.

Response header         | Description
------------------------|---
`Retry-After`           | containing the time after which a new request can be made.
`RateLimit-Remaining`   | containing the time remaining before a new request can be made.
`RateLimit-Limit`       | containing the total limit.
`RateLimit-Reset`       | containing the time after which a new request can be made.

## Time Zones

All times are given in UTC.

### Create entry

#### Required fields

Name                | Type      | Description
--------------------|-----------|------------
feed_id             | String    |

#### Optional fields

Name                | Type      | Description
--------------------|-----------|------------
title               | String    |
published           | Datetime  | Publication date, e.g. `2020-02-23 14:17:12`
content             | String    | Can contain HTML
url                 | String    |
author              | String    |
visible             | Boolean   |
text_editor         | String    | Can be `ssml` or `text`
position            | Integer   | The lower the value, the earlier this Entry appears in the Feed
image_url           | String    |

#### `</>` Example

##### REQUEST

```shell
curl -s -H "Content-Type: application/json" -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -XPOST \
  -d '{"entry":{"feed_id":"dagelijks-nieuws"}}' \
  https://api.xs2radio.com/api/v1/entries.json
```

##### RESPONSE

```
201 Created
```

```json
{
    "id": "1234",
    "title": "",
    "published": "",
    "content": "",
    "url": "",
    "author": "",
    "character_count": "",
    "created_at": "",
    "updated_at": "",
    "seconds": "0",
    "visible": true,
    "text_editor": "text",
    "feed_id": "dagelijks-nieuws",
    "position": "1",
    "image_url": ""
}
```

### Read entry

#### Required fields

Name                | Type      | Description
--------------------|-----------|------------
feed_id             | String    |
url                 | String    |

## Contact

Questions? Reach out to us via `<info@xs2radio.com>`.
