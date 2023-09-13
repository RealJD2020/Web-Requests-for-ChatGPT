# Web Requests Pro Documentation

Welcome to the Web Requests Pro documentation! This guide provides detailed information on how to use the Web Requests Pro plugin, which offers powerful capabilities for web browsing, API calls, image generation, and more. With Web Requests Pro, you can overcome GPT's knowledge cutoff and access a wide range of content from the web. Below, you'll find descriptions of each endpoint, their parameters, and examples of how to use them effectively.

## Table of Contents
1. [Scrape URL](#scrape-url)
2. [REST API Call](#rest-api-call)
3. [Generate Image](#generate-image)
4. [Create Checkout Session](#create-checkout-session)
5. [Get Wallet Profile](#get-wallet-profile)
6. [Create Playground](#create-playground)
7. [Edit Playground](#edit-playground)
8. [Log Playground](#log-playground)
9. [Get System Message](#get-system-message)
10. [Help FAQ](#help-faq)
11. [Promptate Capture Lead](#promptate-capture-lead)
12. [Auth Challenge](#auth-challenge)
13. [Conclusion](#conclusion)

## Scrape URL

**Endpoint: /scrape_url**

This endpoint allows users to browse the web via a URL or perform a web search using provided search terms. It can be used to load web pages, raw text files, PDFs, JSON, XML, CSV, and images. If search terms are provided instead of a URL, it will perform a Google search.

| **Parameter** | **Type** | **Default** | **Description** |
| --- | --- | --- | --- |
| url | string | - | The URL to scrape or perform a web search. If `is_search` is set to true, the URL will be treated as a search query.
| token | string | - | API key or bearer token required for specific requests.
| page | integer | 1 | The page/chunk number to retrieve from a previous Job_ID.
| page_size | integer | 10000 | The maximum number of characters of content to return.
| is_search | boolean | false | Indicates whether the request is a search query.
| num_results_to_scrape | integer | - | Number of search results to return (if is_search is true).
| job_id | string | - | Job ID for pagination and organizing requests.
| refresh_cache | boolean | false | Indicates whether to refresh the cache for the content at the URL.
| no_strip | boolean | false | Indicates whether to skip stripping HTML tags and clutter from the content.
| pro | boolean | false | Indicates whether to use the Pro version of Web Requests.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the request was successful.
| content | object | The text content from the URL or search results in various formats.
| error | string | An error message, if any.
| has_more | boolean | Indicates whether there are more chunks available for pagination.
| job_id | string | Job ID for pagination.
| cache_age | integer | Age of the cache in seconds.
| page_context | string | Context of the current page/chunk in relation to the total number of pages/chunks.
| notice | string | A system message intended for the AI Assistant to instruct and inform subsequent actions.
| alert | string | An important system message that should be observed and retained in memory.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## REST API Call

**Endpoint: /rest_api_call**

This endpoint enables users to make POST or GET HTTP API calls with optional headers and body. It is a more advanced tool than the `/scrape_url` endpoint, providing greater flexibility for making API requests.

| **Parameter** | **Type** | **Default** | **Description** |
| --- | --- | --- | --- |
| url | string | - | The endpoint to which the API call payload will be sent.
| http_method | string | "POST" | The HTTP method to use for the request (POST or GET).
| chunk | integer | - | The chunk of the response to return for pagination.
| req_id | string | - | The unique request ID of a prior request for pagination.
| payload_headers | string | - | The headers to include in the API call.
| payload_body | string | - | The body of the API request.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the API call was successful.
| content | object | The body of the response from the API request.
| chunk | string | The chunk number of the response.
| has_more | boolean | Indicates whether there are more chunks available for pagination.
| req_id | string | The unique request ID for pagination.
| alert | string | An important system message that should be observed and retained in memory.
| error | string | An error message, if any.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Generate Image

**Endpoint: /generate_image**

This endpoint allows users to generate an image from a provided prompt. Each user gets one free image per day, delivered in 1024x1024 pixels. Additional images can be generated with a paid token.

| **Parameter** | **Type** | **Default** | **Description** |
| --- | --- | --- | --- |
| prompt | string | - | The prompt based on which the image will be generated.
| token | string | - | Pre-paid generation credits come in 100-packs

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the image generation was successful.
| image_url | string | URL of the generated image.
| instructions | string | Instructions for rendering the image.
| error | string | An error message, if any.
| remaining_credits | integer | The number of image generation credits remaining for the user.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Create Checkout Session

**Endpoint: /create_checkout_session**

This endpoint initiates the creation of a Stripe checkout session, allowing users to

 purchase premium Web Requests Pro features.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| url | string | The URL to proceed with the checkout.
| instructions | string | Custom instructions related to the payment.
| token | string | A unique token to include in subsequent requests to track usage.

## Get Wallet Profile

**Endpoint: /get_wallet_profile**

This endpoint retrieves a comprehensive summary of an Ethereum wallet's key stats using the Etherscan API. Users must provide their own Etherscan API key to access the service.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| etherscan_api_key | string | The API key provided by Etherscan for accessing their service.
| ethereum_address | string | The Ethereum address of the wallet for which the profile is being requested.
| req_id | string | The unique request ID of a prior request for pagination.
| chunk | integer | The chunk number of the response to return for pagination.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the request was successful.
| content | object | The profile data of the Ethereum wallet.
| req_id | string | The unique request ID for pagination.
| chunk | string | The context of the current chunk in relation to the total number of chunks.
| cache_age | integer | The age of the cache in seconds since the content was originally fetched.
| has_more | boolean | Indicates whether there are more chunks available for pagination.
| error | string | An error message, if any.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Create Playground

**Endpoint: /create_playground**

This endpoint allows users to create a new p5.js playground with the specified name and canvas size. A p5.js playground is a directory with an index.html file that loads the p5.js library and the main.js file where the user's code will be placed.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| name | string | The name of the new playground to be created or recovered.
| uuid | string | The UUID of the playground to recover if 'recover_playground' is set to true.
| recover_playground | boolean | If set to true, Web Requests will try to find and return the source of the specified UUID.
| canvas | array | The size of the canvas represented as a tuple of width and height.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| alert | string | An important system message that should be observed and retained in memory.
| success | boolean | Indicates whether the request was successful.
| uuid | string | The UUID of the playground.
| total_lines | integer | The total number of lines of code in the latest revision of the source code.
| source | array | The current state of the code inside main.js.
| name | string | The name of the playground.
| url | string | The URL of the playground's preview page.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Edit Playground

**Endpoint: /edit_playground**

This endpoint allows users to edit the primary 'main.js' client-side JavaScript of an existing p5.js playground. Users can apply a list of actions to modify the p5.js code, such as insertions, replacements, or deletions. This endpoint is especially useful for collaborative coding where Web Requests Pro AI Assistant helps in making code suggestions.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| uuid | string | The UUID of the playground to edit.
| name | string | The name of the playground.
| actions | array | A list of actions to modify the code.
| pro_mode | boolean | Flag to indicate if the request is intended for elevated Web Requests Pro treatment.
| change_id | string | The change ID for collaborative editing (Pro Mode).
| changelog | string | The context or explanation for the actions being submitted (Pro Mode).
| add_reply | string | An additional reply to add context for Web Requests Pro's AI Assistant (Pro Mode).
| preview_commit | boolean | Flag to indicate if the changes should be staged for preview (Pro Mode).
| commit | boolean | Flag to indicate if the preview commit should be written to disk (Pro Mode).

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the playground edit attempt was successful.
| uuid | string | The UUID of the playground.
| name | string | The name of the playground.
| url | string | The URL of the playground's preview page.
| instructions | string | Important context for the AI assistant.
| total_lines | integer | The total number of lines of code in the latest revision of the source code.
| source | array | The current state of the code inside main.js.
| alert | string | An important system message that should be observed and retained in memory.
| error | string | An error message, if any.
| check_logs | string | Instructions to check the user's logs from their local environment.
| change_id | string | The change ID for collaborative editing (Pro Mode).
| pro_mode | boolean | Flag to indicate if the response is from Web Requests Pro.
| staged_commit | array | The staged code prepared to replace main.js (Pro Mode).
| analysis | string | The analysis from the Web Requests Pro coding assistant.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Log Playground

**Endpoint: /log_playground**

This endpoint provides access to the user's output and error logs for a given playground. It is intended to be used by the AI Assistant to provide context for the user's code.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| uuid | string | The UUID of the playground.
| name | string | The name of the playground.
| full_log | boolean | If set to true, provides the full log of the user's code execution.
| reason | string | The reason why checking the logs is necessary.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the playground was successfully logged and checked.
| uuid | string | The UUID of the playground.
| name | string | The name of the playground.
| content | array | The contents of the log file for the playground in chronological order.
| alert | string | An important system message that should be observed and retained in memory.
| error | string | An error message, if any.
| unread_system_message | boolean | Indicates whether there is an important system message waiting.

## Get System Message

**Endpoint: /get_system_message**

This endpoint retrieves important system messages for the user. It should be called whenever `unread_system_message` is `true` in another web_request response.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| message | string | The message intended for the user.
| instructions | string | Additional instructions for the AI assistant.
| sponsored | boolean | Indicates whether the message is sponsored by a third party.

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

## Promptate Capture Lead

**Endpoint: /promptate_capture_lead**

This endpoint captures lead information provided by the user and stores it for future reference. It is designed to facilitate the process of collecting contact information for potential customers or leads.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| name | string | The name of the lead.
| email | string | The email address of the lead.
| phone | string | The phone number of the lead.
| company | string | The company or organization associated with the lead.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| success | boolean | Indicates whether the lead information was successfully captured.
| message | string | A confirmation message regarding the lead capture.
| error | string | An error message, if any.

## Auth Challenge

**Endpoint: /auth_challenge**

This endpoint is used to authenticate with Web Requests Pro. It should only be called when prompted by specific instructions in a prior request.

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| md5_hash | string | This is a hash that is generated from the challenge instructions given in response to a prior request to a Web Request Pro resource.

| **Response** | **Type** | **Description** |
| --- | --- | --- |
| auth_success | boolean | Indicates whether the user has successfully authenticated with Web Requests Pro.
| instructions | string | These instructions are critical to authenticating the user with Web Request Pro.
| error | string | If there was an error.

## Conclusion

Web Requests Pro offers a wide range of capabilities for web browsing, API interactions, image generation, and more. You can use these endpoints to enhance your applications, perform web scraping, and access valuable data from the web. If you have any questions or need assistance, please refer to the documentation or reach out for support.
```
