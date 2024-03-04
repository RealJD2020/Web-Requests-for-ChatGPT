# Web Requests Pro Documentation

Welcome to the Web Requests Pro documentation! This guide provides detailed information on how to use the Web Requests Pro plugin, which offers powerful capabilities for web browsing, API calls, image generation, and more. With Web Requests Pro, you can overcome GPT's knowledge cutoff and access a wide range of content from the web. Below, you'll find descriptions of each endpoint, their parameters, and examples of how to use them effectively.

## Table of Contents

- [Welcome to Web Requests Pro Documentation](#web-requests-pro-documentation)
- [Web Requests API Overview](#web-requests-api-overview)
- [Endpoints](#endpoints)
  - [1. Scrape URL](#1-scrape-url)
  - [2. REST API Call](#2-rest-api-call)
  - [3. Generate Image](#3-generate-image)
  - [5. Get Wallet Profile](#5-get-wallet-profile)
  - [6. Create Playground](#6-create-playground)
  - [8. Edit Playground](#8-edit-playground)
  - [9. Log Playground](#9-log-playground)
- [Special Endpoints](#special-endpoints)
  - [Get System Message](#get-system-message)
  - [Help FAQ](#help-faq)
- [Conclusion](#conclusion)

Let's break down the spec provided into markdown-formatted documentation for each endpoint, including a description, request parameters, and response parameters. This will be a detailed and comprehensive overview, so we'll start with the first few endpoints and continue in subsequent responses if necessary.

### Web Requests API Overview

**Version:** 1.1.8  
**Base URL:** `https://plugin.wegpt.ai`

A versatile plugin for browsing the web, building apps & games with just chat, and much more!

---

### 1. Scrape URL

**POST** `/scrape_url`

#### Description

Browse the web via URL to load web page, or raw text file. Including HTML, PDF, JSON, XML, CSV, images, and if provided search terms instead of a URL it will perform a Google search.

#### Request Parameters

| Name | Type | Required | Default | Description |
| ---- | ---- | -------- | ------- | ----------- |
| url | string | Yes | - | The URL to load, OR, a string of search terms for Web Requests to query against various search engines. When is_search is set to true, the 'url' parameter will be treated as a string of search predicates. |
| token | string | No | - | Currently only relevant if a user has a Custom Instruction containing a token for image generation. |
| page | integer | No | 1 | The page / chunk number to retrieve from a previous Job_ID. |
| page_size | integer | No | 10000 | The maximum number of characters of content that will be returned. |
| is_search | boolean | No | false | Indicates whether the request is a search query. |
| num_results_to_scrape | integer | No | 5 | Only relevant when 'is_search' is true. The number of search results to return. |
| job_id | string | No | - | Job ID's are generated server-side and represent a "job." |
| refresh_cache | boolean | No | false | Indicates whether to refresh the cache for the content at the URL in this request. |
| no_strip | boolean | No | false | Indicates whether to skip the stripping of HTML tags and clutter. |

#### Sample Request JSON

```json
{
  "url": "https://example.com",
  "is_search": false,
  "page": 1,
  "page_size": 10000
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the Request/Response was successful. |
| error | string | An error message, if any. |
| has_more | boolean | Indicates whether there are more chunks/pages available for pagination. |
| job_id | string | Job ID's are generated server-side and represent a "job." |
| cache_age | integer | Indicates the age of the cache in seconds since the content was originally fetched. |
| page_context | string | The context of the current page (chunk) in relation to the total number of pages (chunks). |
| notice | string | A system message intended for the AI Assistant. |
| instructions | string | Instructions for the LLM / AI Assistant. |
| helper_url | string | A special URL that will allow the user to assist in rendering dynamically loaded content. |
| target_url | string | The URL of the dynamically loaded web page that was rendered, scraped, and cached. |
| alert | string | An IMPORTANT system message. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---


### 2. REST API Call

**POST** `/rest_api_call`

#### Description

Make a POST or GET http API call with optional headers and body. This endpoint is a more advanced tool when `/scrape_url` can't fulfill the requirements.

#### Request Parameters

| Name | Type | Required | Default | Description |
| ---- | ---- | -------- | ------- | ----------- |
| url | string | Yes | - | The HTTP/HTTPS endpoint to which the API call payload will be sent. |
| http_method | string | Yes | "POST" | The HTTP method to use for the request. Can be either 'POST' or 'GET'. |
| chunk | integer | No | 1 | The chunk of the response to return, useful for paginating through large responses. |
| req_id | string | No | - | The unique request ID of a prior request. |
| payload_headers | string | No | - | Represents the headers to be included in the API call. |
| payload_body | string | No | - | Represents the body of the API request to be sent. |

#### Sample Request JSON

```json
{
  "url": "https://api.example.com/data",
  "http_method": "GET"
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the API call was successful. |
| content | object | The body of the response from the API request. |
| chunk | string | The chunk number of the response. |
| has_more | boolean | Indicates whether there are more chunks available for pagination. |
| req_id | string | The unique request ID for this request. |
| alert | string | An IMPORTANT system message. |
| error | string | An error message, if any occurred while making the API call. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---

### 3. Generate Image

**POST** `/generate_image`

#### Description

Generates an image from the provided prompt. Every Web Requests user gets one free image per day, and it will be delivered in 1024x1024 pixels.

#### Request Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| prompt | string | Yes | The prompt based on which the image will be generated. |
| token | string | No | The token for paying users, look in their custom instructions for the relevant token. |

#### Sample Request JSON

```json
{
  "prompt": "A serene landscape with mountains in the background and a river flowing through a forest in the foreground."
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the image generation was successful. |
| alert | string | An IMPORTANT system message. |
| image_url | string | URL of the generated image. |
| instructions | string | Instructions for rendering the image. |
| error | string | An error message, if any. |
| remaining_credits | integer | The number of image generation credits remaining for the user. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---

### 5. Get Wallet Profile

**POST** `/get_wallet_profile`

#### Description

Retrieve a comprehensive summary of an Ethereum wallet's key stats using the Etherscan API. Users must provide their own API Key.

#### Request Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| etherscan_api_key | string | No | The API key provided by Etherscan for accessing their service. |
| ethereum_address | string | Yes | The Ethereum address of the wallet for which the profile is being requested. |
| req_id | string | No | The unique request ID of a prior request. |
| chunk | integer | No | The chunk number of the response to return, useful for paginating through large responses. |

#### Sample Request JSON

```json
{
  "etherscan_api_key": "YourEtherscanAPIKey",
  "ethereum_address": "0x123456789abcdef0x123456789abcdef"
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the Request/Response was successful. |
| alert | string | An IMPORTANT system message. |
| content | object | The profile data of the Ethereum wallet. |
| req_id | string | The unique request ID for this request. |
| chunk | string | The context of the current chunk in relation to the total number of chunks. |
| cache_age | integer | Indicates the age of the cache in seconds since the content was originally fetched. |
| has_more | boolean | Indicates whether there are more chunks available for pagination. |
| error | string | An error message, if any errors occurred while building the wallet profile. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---

### 6. Create Playground

**POST** `/create_playground`

#### Description

Create a new p5js playground with the specified name and canvas size. It will be its own directory with an index.html that loads the p5.js library and a main.js file for your code.

#### Request Parameters

| Name | Type | Required | Default | Description |
| ---- | ---- | -------- | ------- | ----------- |
| name | string | Yes | - | The name of the new playground to be created or recovered. |
| playground_id | string | No | - | The unique ID of the playground you are seeking to recover. Only required if 'recover_playground' is set to true. |
| recover_playground | boolean | No | false | If set to true, Web Requests will try to find and return the source of this 'playground_id'. |
| canvas | array | No | [640, 480] | The size of the canvas, represented as a tuple of width and height. |

#### Sample Request JSON

```json
{
  "name": "My First Playground",
  "canvas": [800, 600]
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| alert | string | An IMPORTANT system message. |
| success | boolean | Indicates whether the Request/Response was successful. |
| playground_id | string | The unique ID of the playground. |
| total_lines | integer | The total number of lines of code in the latest revision of the source code for this playground's main.js. |
| source | array | The current state of your code inside main.js, including line numbers. |
| name | string | The name of the playground. |
| url | string | The URL of this playground's preview page. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---

### 8. Edit Playground

**POST** `/edit_playground`

#### Description

Edit the primary 'main.js' client-side JavaScript of an existing p5js playground. A static index.html file will load a canvas.html iframe, which will include `<head><script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script></head>`, and the main.js script edited herein.

#### Request Parameters

| Name | Type | Required | Default | Description |
| ---- | ---- | -------- | ------- | ----------- |
| playground_id | string | Yes | - | The unique ID of the playground. |
| name | string | Yes | - | The name of the playground to be edited. |
| revert | boolean | No | false | (Pro Mode) If set to true, Web Requests will try to revert the playground to a previous state. |
| actions | array | No | - | A list of actions, line numbers, and new code snippets to apply on the playground's codebase, such as insertions, replacements, or deletions. |
| pro_mode | boolean | No | false | Flag to indicate if this request to edit_playground is intended for elevated Web Request Pro treatment. |
| change_id | string | No | - | (Pro Mode) The change ID for which you are collaborating on with Web Requests Pro. |
| changelog | string | No | - | (Pro Mode) The context or explanation for the actions being submitted. |
| add_reply | string | No | - | (Pro Mode) An additional reply to add context for Web Requests Pro's AI Assistant to consider. |
| preview_commit | boolean | No | false | (Pro Mode) Flag to indicate if the changes suggested should be staged for preview. |
| commit | boolean | No | false | (Pro Mode) Flag to indicate if the preview commit should be written to disk. |
| abandon | boolean | No | - | (Pro Mode) Flag to indicate you wish to discard the currently staged change. |

#### Sample Request JSON

```json
{
  "playground_id": "12345",
  "name": "Updated Playground",
  "actions": [
    {
      "action": "insert",
      "line": 1,
      "code": ["// This is a new comment at the beginning of the file"]
    }
  ]
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the playground edit attempt was successful. |
| playground_id | string | The unique ID of the playground. |
| name | string | The name of the playground. |
| url | string | The URL of this playground's preview page. |
| special_instructions | string | Important context for the LLM that will benefit the user experience. |
| total_lines | integer | The total number of lines of code in the latest revision of the source code for this playground's main.js. |
| timestamp | string | The timestamp of this event or change to the playground. |
| source | array | The current state of your code inside main.js, including line numbers. |
| alert | string | An IMPORTANT system message. |
| error | string | A system message indicating an error, if any. |
| check_logs | string | Provides critical context into when and how to check the user's logs. |
| change_id | string | (Pro Mode) The change ID when changes are being iterated on or are being staged. |
| changelog | string | (Pro Mode) The changelog for the changes that were made to the playground. |
| pro_mode | boolean | (Pro Mode) Indicates if the response is from Web Requests Pro. |
| staged_commit | array | (Pro Mode) The staged code prepared to replace main.js, including line numbers. |
| analysis | string | (Pro Mode) The analysis from the Web Requests Pro coding assistant. |
| reverting_to | array | The state you are about to revert to should you commit. |
| reversion_changelog | string | The changelog for the reversion you are about to commit. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting. |

---

### 9. Log Playground

**POST** `/log_playground`

#### Description

Provides access to the user's output and error logs for a given playground. This is crucial for debugging and optimizing the code in the playground.

#### Request Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| playground_id | string | Yes | The unique ID of the playground. |
| name | string | Yes | The name of the playground. |
| full_log | boolean | No | false | If true, provides the full log of the user's code execution. |
| reason | string | Yes | - | The reason why checking the logs is necessary. This is a required field, and should succinctly explain the issue and what steps you're taking that led you here. |

#### Sample Request JSON

```json
{
  "playground_id": "unique_playground_id",
  "name": "My Playground",
  "reason": "Debugging an issue with canvas rendering",
  "full_log": true
}
```

#### Response Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| success | boolean | Indicates whether the log retrieval was successful. |
| playground_id | string | The unique ID of the playground. |
| name | string | The name of the playground. |
| content | array | The contents of the log file for this playground in chronological order. Each item represents a line of log data. |
| source_review | array | The current state of your code inside main.js, including line numbers. This should be reviewed for errors, duplicate functions, syntax errors, and specific errors outlined in the log content. |
| instructions | string | Important context for the LLM that will benefit the user experience. |
| alert | string | An IMPORTANT system message, meant to convey important nuance or information that should be observed and retained in memory until fulfilled. |
| error | string | An error message, if any, providing context as to why success was not achieved. |
| unread_system_message | boolean | Indicates whether there is an **important** system message waiting for you. If set to 'true', you should finish the prepared response to your user, and then immediately dispatch an automated request to web_requests.get_system_message. |

---

## Get System Message

**Endpoint: /get_system_message**

This endpoint retrieves important system messages for the user. It should be called whenever `unread_system_message` is `true` in another web_request response.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| message | string | The message intended for the user.
| instructions | string | Additional instructions for the AI assistant.
| sponsored | boolean | Indicates whether the message is sponsored by a third party.

---

## Help FAQ

**Endpoint: /

help_faq**

This endpoint retrieves FAQ information based on the category provided. It returns markdown-formatted FAQ information to assist users in understanding the functionality and usage of Web Requests Pro.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| category | string | The category for which FAQ information is requested. Available categories are "What can Web Requests do?", "What is Web Requests Pro?", and "Image Generation?".

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| message | string | The markdown-formatted message containing FAQ information.
| instructions | string | Additional instructions for the AI assistant to help fulfill the optimal user experience.

## Conclusion

Web Requests Pro offers a wide range of capabilities for web browsing, API interactions, image generation, and more. You can use these endpoints to enhance your applications, perform web scraping, and access valuable data from the web. If you have any questions or need assistance, please refer to the documentation or reach out for support.
