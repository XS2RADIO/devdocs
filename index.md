Our API allows you to interact with us using software. The API is RESTful and uses JSON to transport information. Use cases include the automatic creation of entries.

This API documentation will get you started with your first requests with our API. All requests require an active API Token, which you create yourself in our CMS.

## Endpoints

All endpoints use `https://api.xs2radio.com` as the base URL.

Feeds||Description
|-|-|-|
`/v1/feeds.json`  | GET  | [List all feeds](#list-feeds)

Entries||Description
|-|-|-|
`/v1/feeds/asia/entries.json`  | POST  | [Create an entry](#create-entry)
`/v1/feeds/asia/entries.json`  | GET  | [List all entries](#list-entries)
`/v1/feeds/asia/entries/123.json`  | GET  | [Read an entry](#read-entry)
`/v1/feeds/asia/entries/123.json`  | PATCH  | [Update an entry](#update-entry)
`/v1/feeds/asia/entries/123.json`  | DELETE  | [Delete an entry](#delete-entry)

### Authentication

To access our API, you need to include your token in request headers. Treat your token with care! It is a secret that must not be given to anyone. You can store a token, as long as the general public cannot access it directly.

A valid token for your account must be included in every request's `Authorization` header.

### Response codes

Code                |Description
--------------------|---
`200 OK`            | The action was successful
`201 Created`       | The resource was created
`400 Bad Request`   | Missing or invalid fields
`401 Authorization Required`           | Missing or invalid API token
`500 Internal Server Error`              | A problem occurred on our side; please contact us to get it resolved

### Throttling

Usage is unlimited within the subscription and permissions of the account you are using. To prevent fraud and abuse, requests to the API are throttled. You can request the API 150 times each 5 minutes. If you go over, you will see an error response until enough time has elapsed.

Response header         | Description
------------------------|---
`Retry-After`           | containing the time after which a new request can be made.
`RateLimit-Remaining`   | containing the time remaining before a new request can be made.
`RateLimit-Limit`       | containing the total limit.
`RateLimit-Reset`       | containing the time after which a new request can be made.

### Time Zones

All times are given in UTC.


### List Feeds

Useful to enumerate the available URL slugs when working with entries.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X GET \
  https://api.xs2radio.com/api/v1/feeds.json
```

##### RESPONSE

```
200 OK
```

```json
[
  {"id":27, 
  "name":"Joop", 
  "url":"https://joop.bnnvara.nl/feed", 
  "slug":"joop", 
  "visible":false, 
  "created_at":"2021-02-17 13:43:20", 
  "updated_at":"2021-02-17 13:43:20", 
  "language":"nl_NL"
  }
]
```

### Create entry

#### Optional fields

Name                | Type      | Description
--------------------|-----------|------------
`entry[title]`        | String    |
`entry[published]`    | Datetime  | Publication date, e.g. `2020-02-23 14:17:12`
`entry[content]`      | String    | Can contain HTML
`entry[url]`          | String    |
`entry[author]`       | String    |
`entry[visible]`      | Boolean   |
`entry[text_editor]`  | String    | Can be `ssml` or `text`
`entry[position]`     | Integer   | The lower the value, the earlier this Entry appears in the Feed
`entry[image_url]`    | String    |

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X POST \
  -d '{"entry":{"title":"<s>Tentoonstelling World Press</s>"}}' \
  https://api.xs2radio.com/api/v1/feeds/asia/entries.json
```

##### RESPONSE

```
201 Created
```

```json
{
  "id": "123",
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
  "image_url": "",
  "audio_url": "https://www..."
}
```

### Read entry

Get the entry's attribute values.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" 
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X GET \
  https://api.xs2radio.com/api/v1/feeds/asia/entries/123.json
```

##### RESPONSE

```
200 OK
```

```json
{
    "id": "123",
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
    "image_url": "",
    "audio_url": "https://www..."
}
```
### List Entries

Useful to enumerate the available Entry Id's when working with entries.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X GET \
  https://api.xs2radio.com/api/v1/feeds/asia/entries.json
```

##### RESPONSE

```
200 OK
```

```json
[
  {"id":27, 
  "name":"Joop", 
  "url":"https://joop.bnnvara.nl/feed", 
  "slug":"joop", 
  "visible":false, 
  "created_at":"2021-02-17 13:43:20", 
  "updated_at":"2021-02-17 13:43:20", 
  "language":"nl_NL"
  }
]
```

### Update entry

See [Create Entry](#create-entry) for the fields.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X PATCH \
  -d '{"entry":{"title":"<s>Tentoonstelling World Press</s>"}}' \
  https://api.xs2radio.com/api/v1/feeds/asia/entries/22949.json
```

##### RESPONSE

```
200 OK
```

```json
{
  "id":22949, 
  "title":"<s>Tentoonstelling World Press</s>", 
  "published":"2021-02-17T11:44:45.000Z", 
  "content":"<s>De tentoonstelling van World Press Photo in de Nieuwe Kerk in Amsterdam gaat open op zaterdag 17 april.</s><s>Dat is twee dagen na de bekendmaking van de winnaars van de jaarlijkse persfotowedstrijd.</s><s>Belangstellenden kunnen de foto's tot en met zondag 25 juli in de kerk bekijken, maar alleen met een gereserveerd ticket.</s>", 
  "url":"d5f040ef3b625041b156331ba406c188", 
  "author":"ANP Producties", 
  "character_count":nil, 
  "created_at":"2021-02-17T11:50:51.823Z", 
  "updated_at":"2021-02-17T12:30:49.220Z", 
  "feed_id":21, 
  "seconds":33, 
  "visible":true, 
  "text_editor":"ssml", 
  "position":21385, 
  "image_url":"https://www.anpfoto.nl/search.pp?ShowPicture=411698522"
}
```

### Delete entry

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X DELETE \
  https://api.xs2radio.com/api/v1/feeds/asia/entries/22949.json
```

##### RESPONSE

```
200 OK
```

```json
{
  "id":22949, 
  "title":"<s>Tentoonstelling World Press</s>", 
  "published":"2021-02-17T11:44:45.000Z", 
  "content":"<s>De tentoonstelling van World Press Photo in de Nieuwe Kerk in Amsterdam gaat open op zaterdag 17 april.</s><s>Dat is twee dagen na de bekendmaking van de winnaars van de jaarlijkse persfotowedstrijd.</s><s>Belangstellenden kunnen de foto's tot en met zondag 25 juli in de kerk bekijken, maar alleen met een gereserveerd ticket.</s>", 
  "url":"d5f040ef3b625041b156331ba406c188", 
  "author":"ANP Producties", 
  "character_count":nil, 
  "created_at":"2021-02-17T11:50:51.823Z", 
  "updated_at":"2021-02-17T12:30:49.220Z", 
  "feed_id":21, 
  "seconds":33, 
  "visible":true, 
  "text_editor":"ssml", 
  "position":21385, 
  "image_url":"https://www.anpfoto.nl/search.pp?ShowPicture=411698522"
}
```

## Contact

Questions? Reach out to us via `<info@xs2radio.com>`.
