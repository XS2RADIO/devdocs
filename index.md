Our API allows you to interact with us using software. The API is RESTful and uses JSON to transport information. Use cases include the automatic creation of entries.

This API documentation will get you started with your first requests with our API. All requests require an active API Token, which you create yourself in our CMS or it will be provided to you by your XS2Content accountmanager.

## Endpoints

All endpoints use `https://api.xs2radio.com/v1` as the base URL.

Feeds||Description
|-|-|-|
`/feeds.json`  | GET  | [List all feeds](#list-feeds)

Entries||Description
|-|-|-|
`/feeds/<slug>/entries.json`  | POST  | [Create an entry](#create-entry)
`/feeds/<slug>/entries.json`  | GET  | [List all entries](#list-entries)
`/feeds/<slug>/entries/<id>.json`  | GET  | [Read an entry](#read-entry)
`/feeds/<slug>/entries/<id>.json`  | PATCH  | [Update an entry](#update-entry)
`/feeds/<slug>/entries/<id>.json`  | DELETE  | [Delete an entry](#delete-entry)

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

Useful to enumerate the available URL slugs when working with entries. Besides that the response will contain all attributes of a feed, which can be helpful to checkout settings of a feed. For example: you can determine the language that is being used for the Text-To-Speech conversion.

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
  {
  "id": 30,
  "name": "Joop",
  "url": "https://joop.bnnvara.nl/feed",
  "description": "",
  "slug": "joop",
  "visible":false,
  "created_at":"2021-02-17 13:43:20",
  "updated_at":"2021-02-17 13:43:20",
  "language":"nl_NL",
  ...
  }
]
```

### Create entry

Create a new entry. A title or content will give a proper output, but are not mandatory. When an entry is created, a mediafile (audio/video) will be asynchronously be created, this will take up to a minuted for an audiofile, a few minutes for a video file and can take 10 to 30 minutes for avatar feeds.

#### Optional fields

Name                | Type      | Description
--------------------|-----------|------------
`entry[title]`        | String    |
`entry[published]`    | Datetime  | Publication date, e.g. `2020-02-23 14:17:12`
`entry[content]`      | String    | Can contain HTML
`entry[url]`          | String    | This can be a unique url or another external uid
`entry[author]`       | String    |
`entry[visible]`      | Boolean   | This only affects the visibility in the CMS, so in most cases not useful
`entry[text_editor]`  | String    | Can be `ssml` or `text`
`entry[position]`     | Integer   | The lower the value, the earlier this Entry appears in the Feed
`entry[image_url]`    | String    | An image can be used for the XS2C player, Omnystudio player, Spotify, Podimo and others
`entry[video_url]`    | String    | Can only be used if you have a feed that converts a video
`entry[music_layer_0_url]`    | String    | For adding music or sound to a video conversion
`entry[music_layer_1_url]`    | String    | For adding music or sound to a video conversion
`entry[music_layer_2_url]`    | String    | For adding music or sound to a video conversion
`entry[srt_url]`      | String    | For adding subtitles to a video creation or conversion

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X POST \
  -d '{"entry":{"title":"<s>Tentoonstelling World Press</s>","content":"<s></s>"}}' \
  https://api.xs2radio.com/api/v1/feeds/joop/entries.json
```

##### RESPONSE

```
201 Created
```

```json
{
  "id": 22949,
  "title": "<s>Tentoonstelling World Press</s>",
  "published": "",
  "content": "",
  "url": "",
  "author": "",
  "character_count": "",
  "created_at": "",
  "updated_at": "",
  "seconds": "0",
  "visible": false,
  "text_editor": "text",
  "feed_id": 30,
  "position": "1",
  "image_url": "",
  "audio_url": "https://www..."
  ...
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
  https://api.xs2radio.com/api/v1/feeds/joop/entries/22949.json
```

##### RESPONSE

```
200 OK
```

```json
{
    "id": 22949,
    "title": "<s>Tentoonstelling World Press</s>",
    "published": "",
    "content": "",
    "url": "",
    "author": "",
    "character_count": "",
    "created_at": "",
    "updated_at": "",
    "seconds": "0",
    "visible": false,
    "text_editor": "text",
    "feed_id": "30",
    "position": "1",
    "image_url": "",
    "audio_url": "https://www...",
    ...
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
  https://api.xs2radio.com/api/v1/feeds/joop/entries.json
```

##### RESPONSE

```
200 OK
```

```json
[
  {
    "id": 22949,
    "title": "<s>Tentoonstelling World Press</s>",
    "published": null,
    "content": null,
    "url": null,
    "author": null,
    "character_count": null,
    "created_at": "2021-12-16T13:23:41.562Z",
    "updated_at": "2021-12-16T13:23:41.569Z",
    "feed_id": 30,
    "seconds": 0,
    "visible": true,
    "text_editor": "text",
    "position": 1,
    "image_url": null,
    "original_title": null,
    "original_content": null,
    "text_moderated": false,
    "video_url": null,
    "music_layer_0_url": null,
    "music_layer_1_url": null,
    "music_layer_2_url": null,
    "srt_url": null,
    "custom_id": null
  },
  {
    "id": 24251,
    "title": "5. Eilandwachter",
    "published": "2021-03-12T11:01:00.000Z",
    "content": "Op vijf: Eilandwachter!\r\n\r\nDe boswachter kennen we natuurlijk allemaal, maar stel dat je betaald toezicht mag houden op een prachtig eiland op de Bahama's? Luieren op strand is er alleen niet bij. Het is hard werken en vooral veel onderhoud plegen op het eiland voor maximaal 100-duizend euro per jaar.",
    "url": null,
    "author": null,
    "character_count": 317,
    "created_at": "2021-03-12T09:54:59.815Z",
    "updated_at": "2021-03-12T10:06:22.638Z",
    "feed_id": 30,
    "seconds": 29,
    "visible": false,
    "text_editor": "ssml",
    "position": 1,
    "image_url": "",
    "original_title": "5. Elandwachter",
    "original_content": "Op vijf: Eilandwachter!\r\n\r\nDe boswachter kennen we natuurlijk allemaal, maar stel dat je betaald toezicht mag houden op een prachtig eiland op de Bahama's? Luieren op strand is er alleen niet bij. Het is hard werken en vooral veel onderhoud plegen op het eiland voor maximaal 100-duizend euro per jaar.",
    "text_moderated": false,
    "video_url": null,
    "music_layer_0_url": null,
    "music_layer_1_url": null,
    "music_layer_2_url": null,
    "srt_url": null,
    "custom_id": null
  }
]
```

### Update entry

See [Create Entry](#create-entry) for the fields. After the update, the original file will be updated. As with the creation of an entry, this will take up to a minuted for an audiofile, a few minutes for a video file and can take 10 to 30 minutes for avatar feeds.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X PATCH \
  -d '{"entry":{"content":"<s>De tentoonstelling van World Press Photo in de Nieuwe Kerk in Amsterdam gaat open op zaterdag 17 april.</s><s>Dat is twee dagen na de bekendmaking van de winnaars van de jaarlijkse persfotowedstrijd.<s><s>Belangstellenden kunnen de fotos tot en met zondag 25 juli in de kerk bekijken, maar alleen met een gereserveerd ticket.","url":"d5f040ef3b625041b156331ba406c188","author":"ANP Producties"}}' \
  https://api.xs2radio.com/api/v1/feeds/joop/entries/22949.json
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
  "content":"<s>De tentoonstelling van World Press Photo in de Nieuwe Kerk in Amsterdam gaat open op zaterdag 17 april.</s><s>Dat is twee dagen na de bekendmaking van de winnaars van de jaarlijkse persfotowedstrijd.</s><s>Belangstellenden kunnen de fotos tot en met zondag 25 juli in de kerk bekijken, maar alleen met een gereserveerd ticket.</s>",
  "url":"d5f040ef3b625041b156331ba406c188",
  "author":"ANP Producties",
  "character_count":null,
  "created_at":"2021-02-17T11:50:51.823Z",
  "updated_at":"2021-02-17T12:30:49.220Z",
  "feed_id":30,
  "seconds":33,
  "visible":false,
  "text_editor":"ssml",
  "position":21385,
  "image_url":"https://www.anpfoto.nl/search.pp?ShowPicture=411698522",
  ...
}
```

### Delete entry

Delete an entry and its mediafiles.

#### `</>` Example

##### REQUEST

```shell
curl -s \
  -H "Content-Type: application/json" \
  -H "Authorization: Token pX27zsMN2ViQKta1bGfLmVJE" \
  -X DELETE \
  https://api.xs2radio.com/api/v1/feeds/joop/entries/22949.json
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
  "content":"<s>De tentoonstelling van World Press Photo in de Nieuwe Kerk in Amsterdam gaat open op zaterdag 17 april.</s><s>Dat is twee dagen na de bekendmaking van de winnaars van de jaarlijkse persfotowedstrijd.</s><s>Belangstellenden kunnen de fotos tot en met zondag 25 juli in de kerk bekijken, maar alleen met een gereserveerd ticket.</s>",
  "url":"d5f040ef3b625041b156331ba406c188",
  "author":"ANP Producties",
  "character_count":null,
  "created_at":"2021-02-17T11:50:51.823Z",
  "updated_at":"2021-02-17T12:30:49.220Z",
  "feed_id":30,
  "seconds":33,
  "visible":true,
  "text_editor":"ssml",
  "position":21385,
  "image_url":"https://www.anpfoto.nl/search.pp?ShowPicture=411698522",
  ...
}
```

## Contact

Questions? Reach out to us via `<info@xs2content.com>`.
