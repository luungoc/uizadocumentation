# Uiza APIs Documentation

## Introduction

Welcome to Uiza APIs documentation! Uiza helps you build powerful video platforms by providing easy-to-use APIs that follow [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) standards. Our APIs have predictable, resource-oriented URLs, accept [encoded-form](https://en.wikipedia.org/wiki/POST_(HTTP)#Use_for_submitting_web_forms) request bodies, use [authentication](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#authentication), verbs, and return [JSON-Objects](https://www.json.org/) with [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

*Important: You cannot run the sample requests in this guide as-is. Replace call-specific parameters such as `tokens` and `AppIDs` with your own values. See [Generate API Key]() to obtain your own keys.*

Was this section helpful? [Yes]() [No]()

## Authentication

The Uiza APIs use [API keys](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#generate-api-key) to authenticate requests. You can view and manage your [API keys](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#generate-api-key) in the [Uiza Dashboard](https://dashboard.uiza.io/).

Your [API keys](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#generate-api-key) contain rights and permissions to your account; therefore, make sure to keep them secure. Do not share your [API keys](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#generate-api-key) in publicly accessible areas such as GitHub, client-side code, and so forth.

Was this section helpful? [Yes]() [No]()

### Generate API Key

**Step 1**: [Login](https://account.uiza.io/) to your workspace. If you don't have an account yet, sign up [here](https://account.uiza.io/register).  

![Screen 0](./screen0.png)

**Step 2**: Navigate to the **APPLICATION SETTINGS** section, select **Publish API**. An API key is provided to you by default.  

![Screen 1](./screen1.png)

**Step 3**:  To generate your own key, click the **+New** button, specify the key's name (API name), and select **CREATE**.

![Screen 2](./screen2.png)

### Use API Key to Authenticate Your API Requests

Authentication to the API is performed via [HTTP Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). Provide your [API key](https://github.com/luungoc/uizadocumentation/blob/v2/UizaDocumentation.md#generate-api-key) as the basic auth `username` value. You do not need to provide a password for authentication.

If you need to authenticate via [bearer auth](https://tools.ietf.org/html/rfc6750) (e.g., for a cross-origin request), use `-H "Authorization: Bearer uap-5126204af4ee487f9c295525f03b9268-353340ef"` instead of `-u uap-5126204af4ee487f9c295525f03b9268-353340ef`.

Was this section helpful? [Yes]() [No]()

## Request Body

Uiza APIs accept request arguments as a [JSON-Object](https://www.json.org/) in the HTTP request body.

Was this section helpful? [Yes]() [No]()

## Error Codes

Uiza uses standard [HTTP response status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate the status of an API request. In general:  

* Codes in the `2xx` range indicate a success.

* Codes in the `4xx` range indicate an error that the request failed given the information provided (e.g., a required parameter was omitted, a charge failed, etc.).  

* Codes in the `5xx` range indicate an error with Uiza's servers.

### Error List

Error Code | Description
------------ | -------------
200 - OK | The request was successful.
400 - Bad Request | The request was unacceptable, often due to missing a required parameter.
401 - Unauthorized | No valid API key provided.
404 - Not Found | The request resource doesn't exist.
422 - Unprocessable | The syntax of the request is incorrect, often due to invalid parameter(s)
500 - Internal Server Error | There is a problem with the Uiza server. Try again later.
501 - Service Unavailable | The Uiza server is overloaded or is down for maintenance.

## Pagination

All top-level API resources support bulk fetches via **list** APIs. For instance, you can [list entities](), [list categories](), and [list invoices](). These list APIs share a common structure, taking at least these two parameters: `limit`, `page`.

### List Response Format

Attribute | Type | Description
------------ | ------------- | -------------
data | string | An array containing the actual response elements, paginated by any request parameters.
metadata | string | Contains `total` as total number of data without pagination, `result` as number of paginated data, `page` as current page, `limit` as how many records each array of data contains.

## Request IDs

Each API request has an associated request identifier. You can find this value as the `u-request-id` (Uiza's Request ID) in the response header. **If you need to contact us about a specific request, providing the request identifier will ensure the fastest possible resolution.**

Was this section helpful? [Yes]() [No]()

## Video

Uiza calls videos as `Entities`. You can create entities by uploading media files using [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), and [S3](https://en.wikipedia.org/wiki/Amazon_S3) links. The Video APIs provide CRUD operations (create, retrieve, update, delete) on the entities . You can retrieve individual entities as well as a list of all of your entities.

All videos (`entities`) need to be [published]() before the public can view them. Unpublished videos can still be [edited]() and [listed]() by APIs or using [Dashboard](https://dashboard.uiza.io/), but the [Player]() cannot play them.

Was this section helpful? [Yes]() [No]()

### The Entity Object

Attribute | Type | Description | Note
------------ | ------------- | ------------- | -------------
id | string | The unique identifier of the entity (video) object |
name | string | The video's name | Displayed to the end users
url | text | The full URL of the media file (direct public [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), and [AWS S3](https://en.wikipedia.org/wiki/Amazon_S3)) | Send **an empty string** if you upload with the AWS SDK.
inputType | enum | The type of URL. Allowed values: `http, s3, ftp, s3-uiza`. | In case `url` is **an empty string**, this must be `s3-uiza`.
description | text | The full description of the video.  (at most 65,535 characters) | Displayed to the end users
metadataId | array | medataId is [CategoryId]() |
shortDescription | text | The short description of the video (at most 250 characters)| Displayed to the end users
poster | string | The video poster's URL | Displayed to the end users
thumbnail | string | The video's [thumbnail](https://en.wikipedia.org/wiki/Thumbnail) URL | Displayed to the end users
type | enum | Has 2 types: `VOD` and `AOD` | `VOD` stands for [Video on Demand](https://en.wikipedia.org/wiki/Video_on_demand). `AOD` stands for [Audio on Demand](https://www.muvi.com/wiki/audio-on-demand.html)
duration | string | The duration of the entity in seconds (at most one million seconds) | Displayed to the end users
metadataIds | array | A list of categories of this entity | | No
extendMetadata | object | A set of key-value pairs as additional information that you can attach to a video object | Displayed to the end users
embedMetadata | object | A set of predefined key-value pairs that you can attach to a video object, meant to be displayable to end users. See [Embed Metadata]().
view | number | The total times that the video has been viewed
publishpublishoCdn | string | The status of the publishing task. Allowed values: `queue`, `not-ready`, `success`, `failed`
createdAt | datetime | The time at which the entity was created, in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime-970915)|
editedAt | datetime | The time at which the entity was last edited, in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime-970915)|

### Create an Entity

Creates an entity with a full URL, which can be a direct [HTTP/HTTPS](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), and [S3](https://en.wikipedia.org/wiki/Amazon_S3) link.

#### The Entity Object

Parameter | Type | Description | Note |Required
------------ | ------------- | ------------- | ------------- | -------------
name | string | The name of the entity | | Yes
url | text | The full URL of the media file (direct public [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol), [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), hyper links, and [AWS S3](https://en.wikipedia.org/wiki/Amazon_S3)) | Send **an empty string** if you upload with SDK | Yes
inputType | enum | The type of URL. Allowed values: `http, s3, ftp, s3-uiza`. | In case `url` is **an empty string**, this must be `s3-uiza` | Yes
description | text | The full description of the video to be displayed to the end users (at most 65,535 characters) | | No
metadataId | array | medataId is [CategoryId]() | | No
shortDescription | text | The short description of the video (at most 250 characters)| Displayed to the end users | No
poster | string | The video poster's URL | Displayed to the end users | No
thumbnail | string | The video's [thumbnail](https://en.wikipedia.org/wiki/Thumbnail) URL | Displayed to the end users | No
metadataIds | array | A list of categories of this video | | No
extendMetadata | object | A set of key-value pairs as additional information that you can attach to a video object | Displayed to the end users | No
embedMetadata | object | A set of predefined key-value pairs that you can attach to a video object. See [Embed Metadata](). | Displayed to the end users | No

*Note: If you want to integrate with Uiza using the AWS SDK to upload your media files to Uiza storage, please leave parameter `url` **empty**, and set parameter `inputType` to `s3-uiza`.*

#### Response

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created entity

Was this section helpful? [Yes]() [No]()

### Live Streaming

Live streaming is managed by Uiza's `Live Event`. A live event allows you to **pull** contents from other sources such as [Youtube](https://www.youtube.com/) or any [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) link. You can also broadcast a live stream with a **push** type of live event.

A live event can be recorded. These records are save as [entities]().

To start a live stream, you must create a live event first.

***IMPORTANT**: Live minutes start counting as soon as the live event starts.*

Was this section helpful? [Yes]() [No]()

#### The Live Event Object

Attribute | Type | Description | Note
------------ | ------------- | ------------- | -------------
name | string | The name of the event (at most 100 characters) |
mode | string | The type of the event. Allowed values: `pull | push` |**Pull** link can either be a [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) URL or a .m3u8 URL ([HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)) or any Youtube link (live or not). **Push**: Uiza will generate an end point, which can be used in broadcasting softwares such as [OBS](https://obsproject.com/) to start your streaming.
encode | string | Set to `1` to enable live transcoding. Live transcoding will enable [Adaptive Bitrate](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) on the end user's side which would help deliver a better viewing experience. When set to `0`, the end users will watch with source bitrate. |
dvr | enum | Indicates whether to record a live stream. If set to `1`, a [mp4](https://en.wikipedia.org/wiki/MPEG-4_Part_14) file will be generated after the live stream ends. Set to `0` to disable this feature. |
linkPublishSocial | array | Live feed will be posted on this set of social media links.
poster | string | The video's poster `URL` | Displayed to the end users
thumbnail | string | The [thumbnail](https://en.wikipedia.org/wiki/Thumbnail)'s `URL` | Displayed to the end users
channelName | string | The key name of the channel. | Yes
createdAt | datetime | The time at which the event was created, in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime-970915)|
editedAt | datetime | The time at which the event was last edited, in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime-970915)|

#### Create a Live Event

You can create a new live event to manage a live stream.

***IMPORTANT**: Live minutes start counting as soon as the live event starts.*

#### The Live Event Object

Parameter | Type | Description | Note | Required
------------ | ------------- | ------------- | ------------- | -------------
name | string | The name of the event (at most 100 characters) | | Yes
mode | string | The type of the event. Allowed values: `pull`, `push` | **Pull** link can either be a [RTMP](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol) URL, or a .m3u8 URL ([HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)), or any Youtube link (live or not). **Push**: Uiza will generate an end point, which can be used on broadcasting softwares such as [OBS](https://obsproject.com/) to start your streaming | Yes
encode | string | Set to `1` to enable live transcoding. Live transcoding will enable [Adaptive Bitrate](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) on the viewer's side which would help deliver a better viewing experience. When set to `0`, the end users will watch with source bitrate. | | Yes
dvr | enum | Indicates whether to record a live stream. If set to `1`, a [mp4](https://en.wikipedia.org/wiki/MPEG-4_Part_14) file will be created after the live stream ended. Set to `0` to disable this feature. | | Yes
linkPublishSocial | array | Live feed will be posted on this set of social media links.
thumbnail | string | The thumbnail's `URL` | Displayed to the end users
channelName | string | The key name of the channel. | |
linkStream | array | A list of streamable sources that will be used when mode = pull.| | Yes
resourceMode | enum | Resource mode ( single = only 1 feed & output), redundant = more than 1 feed & output to backup)| | Yes

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created event