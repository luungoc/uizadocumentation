# Uiza APIs Documentation

## Introduction

Welcome to Uiza APIs documentation! Uiza helps you build powerful video platforms by providing easy-to-use APIs that follow RESTful standards. Our APIs have predictable, resource-oriented URLs, and use HTTP response status codes to indicate API errors. A JSON object is returned by all API responses, including errors.

You can view code examples provided in different programming languages in the code snippet on the right.

## Global Format APIs

Below is the default format of all API responses:

Parameter | Type | Description
------------ | ------------- | -------------
data | Object | An array (mutiple items), or an object (single item)
version | Number | The version of the API
dateTime | Datetime | The response time of the API (GTM+0)
policy | String | The type of the calling method (public)
requestId | String | The response ID of the request
serviceName | String | The service name
message | String | The response message
code | Number | The Status Reponse code
type | String | The result of the API call

*Note:* The `requestId` should be included in your email or your chat for faster support from Uiza.

## Authentication

The Uiza APIs use **API keys** to authenticate requests. You can view and manage your **API keys** in the **Uiza Dashboard**.

Your **API keys** contain rights and permissions to your account; therefore, make sure to keep them secure. Do not share your API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

Authentication to the API is performed via HTTP Basic Authorization. Provide your API key as the basic auth username value. You do not need to provide a password for authorization.

If you need to authenticate via bearer authorization (e.g., for a cross-origin request), use `-H "Authorization: Bearer uap-5126204af4ee487f9c295525f03b9268-353340ef"` instead of `-u uap-5126204af4ee487f9c295525f03b9268-353340ef`.

### Generate API Key

**Step 1**: Login to your workspace.  
**Step 2**: Navigate to the **APPLICATION SETTINGS** section, select **Publish API**. An API key is provided to you by default.  

![Screen 1](./screen1.png)

**Step 3**:  To generate your own key, click the **+New** button, specify the key's name (API name), and select **CREATE**.

![Screen 2](./screen2.png)

## User Management

You can manage users with APIs user. Uiza has 2 levels of user:

Admin - This account level has the highest priority, including permission to create and manage users.

User - This account level is under the Admin level. It only manages the APIs of the account.

### Create a User

Create a user account in your workspace.

#### Header Request

Header | Type | Description | Required
------------ | ------------- | ------------- | -------------
Authorization | String | The API key | Yes

#### Body Request

Parameter | Type | Description | Required
------------ | ------------- | ------------- | -------------
status | integer | The status of the account: (0 = de-active, 1 = active) | Yes
username | string | The username used to log into the account | Yes
email | string | The email used to log into the account | Yes
password | string | The password of the account (from A to z, 6 to 25 characters) | Yes
avatar | string | The link of the avatar (recommending a 300x300px image) | No
fullname | string | The user's fullname| No
dob | date | The user's date of birth (MM/DD/YYYY) | No
gender | integer | The user's gender (0 = Male, 1 = Female)| No
isAdmin | integer | Indicates whether the user is an administrator or not (0: No, 1: Yes) | No

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created user

### Retrieve a User

Retrieve a user using their corresponding identifier, which is returned upon creation.

#### Header Request

Header | Type | Description | Required
------------ | ------------- | ------------- | -------------
Authorization | String | The API key | Yes

#### Body Request

Parameter | Type | Description | Required
------------ | ------------- | ------------- | -------------
id | string | The identifier of the user | Yes

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the user
isAdmin | integer | Indicates whether the user is an administrator or not (0: No, 1: Yes)
username | string | The username used to log into the account | Yes
email | string | The email used to log into the account
createdAt | datetime | The time when the user was created
updatedAt | datetime | The time when the user was updated

## Video

The following APIs are used to create and manage your media file(s), which we called `entity/entities`.

### Create an Entity

Create an entity with a full URL. Direct HTTP/HTTPS, FTP, and AWS S3 links are acceptable.

#### Header Request

Header | Type | Description | Required
------------ | ------------- | ------------- | -------------
Authorization | String | The API key | Yes

#### Body Request

Parameter | Type | Description | Note |Required
------------ | ------------- | ------------- | ------------- | -------------
name | string | The name of the entity | | Yes
url | text | The full URL of the media file (direct public HTTP/HTTPS, FTP, AWS S3) | Send an empty string in case of integration using AWS SDK to upload to your media file to Uiza's storage | Yes
inputType | enum | The type of URL. Allowed values: `http, s3, ftp, s3-uiza`. | In case `url` is empty string, this must be `s3-uiza` | Yes
description | text | The full description of the entity (no length restriction) | | No
metadataId | array | Add relation between entity and folder/playlist | | No
shortDescription | text | The short description of the entity (at most 250 characters) | | No
poster | string | The poster of the entity | | No
thumbnail | string | The thumbnail of the entity | | No
metadataIds | array | List of categories will be attached with the entity | | No
extendMetadata | string | Additional information of the entity | You can input additional information of the entity using the [ key : value ] format. All information will be shown in the entity's detail. | No
embedMetadata | string | See Embedded Metadata for more information | | No

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created entity

### Category

Create an entity category for easier management. A category is used to group all entities of the same type into a folder, playlist, or tag.

#### Header Request

Header | Type | Description | Required
------------ | ------------- | ------------- | -------------
Authorization | String | The API key | Yes

#### Body Request

Parameter | Type | Description |Required
------------ | ------------- | ------------- | -------------
name | string | The name of the category | Yes
type | enum | The type of the category. Allowed values: `folder, playlist, tag` | Yes
description | string | The description of the category | Yes
orderNumber | integer | The order number of the category. The lower the number, the higher the order | No
icon | array | Small icon of the category | No

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created category

### Live Streaming

The following APIs are used to create and manage live streaming events.  

*Notes:*  
A `Live` not started yet is called an `Event`.  
An `Event` already started is called a `Feed`.

#### Create a Live Event

Create a live streaming event. A live stream can be setup and started later, or can be setup and started right after setting up. The `Live Channel Minutes` count increases when the event starts.  

#### Header Request

Header | Type | Description | Required
------------ | ------------- | ------------- | -------------
Authorization | String | The API key | Yes

#### Body Request

Parameter | Type | Description | Note | Required
------------ | ------------- | ------------- | ------------- | -------------
name | string | The name of the event (at most 100 characters) | | Yes
mode | string | The type of the event. Allowed values: `pull, push` | Pull: We support RTMP, HLS and direct Live Youtube link. Uiza pulls feed from the pull link and broadcasts it using the Uiza SDK. Push: Uiza gives you a Publish endpoint, you can push feed into the endpoint, and Uiza will broadcast it using the Uiza SDK. | Yes
encode | string | The mode of the live stream (0 = no encode, 1 = encode) | | Yes
dvr | enum | The feed after streamed will be recorded as a mp4 file (0 = No record, 1 = Active Feature record) | | Yes
description | text | The description of the live stream | | No
linkPublishSocial | array | The information to share the live on social media: `dropdown`: type of social media (Youtube, Facebook). `address`: the stream URL. `streamKey`: the stream key correspond to the stream URL.|  | No
thumbnail | text | An image link | | No
linkStream | array | The link streaming if you choose `mode=pull`| | Yes
resourceMode | array | The resource mode ( `single` = only 1 feed & output, `redundant` = more than 1 feed & output to backup) | | Yes

#### Response Parameter

Parameter | Type | Description
------------ | ------------- | -------------
id | string | The identifier of the newly created event

### Error Codes

Uiza uses conventional HTTP response status codes to indicate the success or failure of an API request. In general:  

* Codes in the `2xx` range indicate a success.

* Codes in the `4xx` range indicate an error that the request failed given the information provided (e.g., a required parameter was omitted, a charge failed, etc.).  

* Codes in the `5xx` range indicate an error with Uiza's servers.

#### Error List

Error Code | Description
------------ | -------------
200 - OK | The request was successful.
400 - Bad Request | The request was unacceptable, often due to missing a required parameter.
401 - Unauthorized | No valid API key provided.
404 - Not Found | The request resource doesn't exist.
422 - Unprocessable | The syntax of the request is incorrect, often due to invalid parameter(s)
500 - Internal Server Error | There is a problem with the Uiza server. Try again later.
501 - Service Unavailable | The Uiza server is overloaded or is down for maintenance.
