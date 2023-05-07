# Web Requests for ChatGPT | OpenAPI Specification

## Overview

The Web Request OpenAPI Specification defines the API for the Web Request plugin, which allows users to request content and other data from a URL and return the response data in a variety of formats. ChatGPT can then parse and classify the response data as needed to fulfill its task.

## OpenAPI Specification

### Version

`3.0.0`

### Info

- **Title**: Web Request
- **Version**: 0.1.1
- **Description**: A plugin to request content and other data from a URL and return the response data for a variety of formats, from which ChatGPT can parse and classify as needed to fulfill its task.

### Paths

#### POST /scrape_url

##### Summary

Scrape content/data from a URL, web page, or endpoint.

##### Operation ID

`scrape_url`

##### Request Body

- **Content-Type**: `application/json`
- **Schema**:
  - **type**: `object`
  - **properties**:
    - `url` (string, required): The URL to scrape or perform a Google search if 'is_search' is set to true. When is_search is set to true, the 'url' parameter will be treated as a search query for Google.
    - `page` (integer, optional, default: 1): The page number (of chunks) to retrieve, based on the page_size that was chosen (defaults to 5000). To request subsequent pages, increment the value of the 'page' parameter, and be sure to send job_id for which you are paginating. For example, to request the second page, set 'page' to 2.
    - `page_size` (integer, optional, default: 5000): The maximum number of characters in each page (chunk).
    - `is_search` (boolean, optional, default: false): Indicates whether the request is a search query. If set to true, the 'url' parameter will be treated as a search query for Google.
    - `follow_links` (boolean, optional, default: false): Only relevant when 'is_search' is true. Indicates whether to return the content of the search result's underlying page or just the result's metadata.
    - `num_results_to_scrape` (integer, optional): Only relevant when 'is_search' is true. The number of search results to return. Default is 3.
    - `job_id` (string, optional): The unique job ID for this request. The job ID is used to ensure consistency when requesting chunks from a URL that has been recently scraped. If not provided, a new job ID will be generated. It is recommended to include the same job ID when requesting subsequent chunks from the same URL to retrieve content from the cached snapshot of the original request.
    - `refresh_cache` (boolean, optional, default: false): Indicates whether to refresh the cache for this request. If set to true, a new request will be made and the cache will be updated. The response may be retrieved from an in-memory cache to improve performance. The 'cache_age' property indicates the age of the cache in seconds since the content was last fetched.
    - `no_strip` (boolean, optional, default: false): Indicates whether to skip cleaning up HTML content. Use this flag if you want to preserve the original HTML structure, such as when looking for source code. When 'no_strip' is set to false (default), HTML content will be sanitized and certain tags (e.g., script and style tags) may be removed for security reasons.
  - **required**: `url`

##### Responses

- **200** (Successful operation):
  - **Description**:

Successful operation.
  - **Content-Type**: `application/json`
  - **Schema**:
    - **type**: `object`
    - **properties**:
      - `success` (boolean): Indicates whether the scraping operation was successful.
      - `content` (object): The scraped content from the URL or search results in various formats, including HTML, XML, JSON, CSV, PDF, images, and plain text. The format may vary based on the content type of the URL being scraped. ChatGPT should expect to receive content as a string, list, dictionary, or other appropriate data structure based on the content type. The content may be sanitized for brevity and safety.
      - `error` (string, optional): An error message, if any. Possible error messages include 'Invalid URL', 'Invalid page or page_size', 'Invalid num_results_to_scrape', 'Unsupported content type: {content_type}', and 'Failed to fetch the content'. Often times adjusting parameters resolves these issues.
      - `has_more` (boolean): Indicates whether there are more chunks available for pagination after the current chunk. Increment previous 'page' number and include corresponding 'job_id' to request the next chunk.
      - `job_id` (string): The unique job ID for this request. The job ID is used to ensure consistency when requesting chunks from a URL that has been recently scraped. If not provided, a new job ID will be generated. It is recommended to include the same job ID when requesting subsequent chunks from the same URL to retrieve content from the cached snapshot of the original request.
      - `cache_age` (integer): The response may be retrieved from an in-memory cache to improve performance. The 'cache_age' property indicates the age of the cache in seconds since the content was originally fetched.
      - `page_context` (string): The context of the current page (chunk) in relation to the total number of pages (chunks) of response data for a given job ID. For example, '2/7' means this is the 2nd chunk out of a total of 7 chunks.

## Example Usage

### Scrape URL

**Request:**
```json
POST /scrape_url
Content-Type: application/json

{
  "url": "https://example.com",
  "page": 1,
  "page_size": 5000
}
```

**Response:**
```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "success": true,
  "content": "Example Domain...",
  "has_more": false,
  "job_id": "12345678-1234-1234-1234-123456789abc",
  "cache_age": 0,
  "page_context": "1/1"
}
```

### Google Search

**Request:**
```json
POST /scrape_url
Content-Type: application/json

{
  "url": "Quart framework",
  "is_search": true,
  "num_results_to_scrape": 3
}
```

**Response:**
```json
HTTP/1.1 200 OK
Content-Type: application/json

{
  "success": true,
  "content": ["https://pgjones.gitlab.io/quart/", "https://github.com/pgjones/quart", "https://pypi.org/project/Quart/"],
  "has_more": false,
  "job_id": "12345678-1234-1234-1234-123456789abc",
  "cache_age": 0,
  "page_context": "1/1"
}
```

## Notes

- The Web Request plugin allows users to scrape content and data from a URL, web page, or endpoint, as well as perform Google searches and retrieve search results.
- The plugin supports various content formats, including HTML, XML, JSON, CSV, PDF, images, and plain text. The format of the response may vary based on the content type of the URL being scraped.
- The plugin provides options for pagination, caching, and content sanitization. Users can specify the page number and page size for paginated responses, refresh the cache if needed, and choose whether to skip cleaning up HTML content.
- The plugin uses an in-memory cache to improve performance. The cache age indicates the age of the cache in seconds since the content was last fetched.
- The plugin validates input parameters and provides error messages for invalid or unsupported requests. Users can adjust the parameters to resolve issues.
- The plugin generates unique job IDs for each request to ensure consistency when requesting chunks from a URL that has been recently scraped. Users are recommended to include the same job ID when requesting subsequent chunks from the same URL.
- The plugin's OpenAPI Specification provides a detailed description of the API, including the available endpoints, request parameters, response schema, and example usage. The specification follows the OpenAPI 3.0.0 standard.
