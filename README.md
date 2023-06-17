# 'Web Requests' :: ChaGPT's Web Browser

Web Requests is a versatile plugin for ChatGPT Plus subscribers that allows users to access and parse content from the web, overcoming the "September 2021 Knowledge Cutoff." The plugin is built as an API service layer backend using the Quart server and is accessible via the provided OpenAPI specification, specifically designed for ChatGPT's Plugins model.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Scrape URL Endpoint](#scrape-url-endpoint)
- [Cache Management](#cache-management)
- [Pagination](#pagination)
- [Input Validation](#input-validation)
- [Content Handling](#content-handling)
- [Command Line Interface](#command-line-interface)
- [OpenAPI Specification](#openapi-specification)
- [Best Practices and Usage](#best-practices-and-usage)

## Features

- Scrape URLs and extract content
- Perform Google searches and return search results
- Follow links and extract content from search results
- Cache results to reduce server load and improve response times
- Paginate content to handle large amounts of data
- Validate input parameters
- Handle various content types, including HTML, XML, JSON, CSV, PDF, and images
- Support for images and text-based files
- Content processing for context preservation
- User-friendly command line interface

## Installation

To install and run the plugin, you need to have Python 3.7 or higher installed on your system. The plugin uses the following libraries:

- Quart
- httpx
- BeautifulSoup
- aiofiles
- htmlmin
- defusedxml
- pdfminer
- PIL (Pillow)
- aioconsole
- hypercorn
- googlesearch-python
- markdownify

Install the required libraries using the following command:

```bash
pip install quart httpx beautifulsoup4 aiofiles htmlmin defusedxml pdfminer.six pillow aioconsole hypercorn googlesearch-python markdownify
```

Then, set the required environment variables in a `.env` file:

```
OPENAI_API_KEY=<your_openai_api_key>
```

Finally, run the plugin using the following command:

```bash
python web_requests.py
```

## Scrape URL Endpoint

**Endpoint:** `/scrape_url`

**Method:** `POST`

**Description:** Scrape a URL or perform a Google search and return the content. The content types supported include HTML, PDF, JSON, XML, CSV, and images.

**Input Parameters:**

- `url` (string, required): The URL to scrape or the search query to perform if `is_search` is set to true.
- `page` (integer, optional, default: 1): The page number for pagination, based on the chosen `page_size`.
- `page_size` (integer, optional, default: 10000): The maximum number of characters per page (chunk) for pagination.
- `is_search` (boolean, optional, default: false): Set to `true` if the input is a search query instead of a URL.
- `follow_links` (boolean, optional, default: false): Set to `true` to follow links in search results and extract content.
- `num_results_to_scrape` (integer, optional, default: 3): The number of search results to return if `is_search` is true.

- `job_id` (string, optional): A unique identifier for the request. If not provided, a UUID will be generated.

- `refresh_cache` (boolean, optional, default: false): Set to `true` to force a cache refresh for the request.
- `no_strip` (boolean, optional, default: false): Set to `true` to disable content minification.

**Output:**

- `success` (boolean): Indicates whether the request was successful.
- `content` (object): The extracted content in various formats, including HTML, XML, JSON, CSV, PDF, images, and plain text.
- `error` (string): An error message, if any.
- `has_more` (boolean): Indicates whether there is more content available for pagination.
- `job_id` (string): The unique identifier for the request.
- `cache_age` (integer): The age of the cache in seconds.
- `page_context` (string): The current page context in the format `current_page/total_pages`.
- `notice` (string): A system message providing additional context that may help instruct and inform subsequent actions.

**Example Request:**

```json
{
  "url": "OpenAI",
  "is_search": true,
  "follow_links": false,
  "num_results_to_scrape": 5
}
```

**Example Response:**

```json
{
  "success": true,
  "content": {
    "search_results": [
      {
        "title": "OpenAI",
        "link": "https://openai.com/",
        "description": "OpenAI is an artificial intelligence research laboratory consisting of the for-profit corporation OpenAI LP and its parent company, the non-profit OpenAI Inc."
      },
      {
        "title": "OpenAI - Wikipedia",
        "link": "https://en.wikipedia.org/wiki/OpenAI",
        "description": "OpenAI is an artificial intelligence (AI) research laboratory consisting of the for-profit corporation OpenAI LP and its parent company, the non-profit OpenAI Inc."
      },
      {
        "title": "GitHub - openai",
        "link": "https://github.com/openai",
        "description": "GitHub is where people build software. More than 73 million people use GitHub to discover, fork, and contribute to over 200 million projects."
      },
      {
        "title": "OpenAI - Home | Facebook",
        "link": "https://www.facebook.com/openai",
        "description": "OpenAI, San Francisco, California. 50,772 likes · 1,445 talking about this. We're conducting research to ensure that artificial general intelligence (AGI) benefits all of humanity. We're... "
      },
      {
        "title": "OpenAI - Crunchbase Company Profile & Funding",
        "link": "https://www.crunchbase.com/organization/openai",
        "description": "OpenAI is an artificial intelligence research organization that aims to promote and develop friendly AI that benefits humanity as a whole."
      }
    ]
  },
  "has_more": false,
  "job_id": "987654",
  "cache_age": 120,
  "page_context": "1/1",
  "notice": ""
}
```

## Cache Management

The plugin uses an in-memory cache to store request results and improve response times. The cache is cleaned every 2 minutes, removing items older than 15 minutes. The cache size is limited to 200 items.

## Pagination

The plugin supports pagination to handle large amounts of data. The `page` and `page_size` input parameters control the pagination behavior. The default values are 1 and 10000, respectively.

## Input Validation

The plugin validates input parameters to ensure they are valid and within acceptable ranges. The `validate_input` function checks the input parameters and returns a boolean value indicating whether the input is valid, along with an error message if the input is invalid.

## Content Handling

The plugin can handle various content types, including HTML, XML, JSON, CSV, PDF, and images. It also supports images and text-based files. The `strip_html` function is used to process HTML content, while other content types are handled by their respective functions.

For images, the plugin returns metadata such as format, size, mode, and image URL. For text-based files, the plugin performs transformations to improve readability.

## Command Line Interface

The plugin provides a user-friendly command line interface that allows you to interact with the server and perform various tasks. The available commands are:

- `stats`: Display server statistics, such as requests per hour, cache stats, and ephemeral user requests.
- `purge cache`: Clear the cache.
- `restart`: Restart the server.
- `quit`: Shut down the server.

To use the command line interface, simply type the desired command and press Enter.

## OpenAPI Specification

The Web Requests plugin is accessible through an OpenAPI 3.0.0 specification, which provides a clear and concise way to describe the API service and its available endpoints. This allows developers to understand and interact with the API more effectively. You can find the OpenAPI specification for the Web Requests plugin [here](./openapi.json).

## Best Practices and Usage

1. **Caching:** To reduce server load and improve response times, the plugin caches request results for 15 minutes. When paginating through large content, use the same `job_id` to retrieve content from the cached snapshot of the original request.

2. **Pagination:** When dealing with large volumes of data, use the `page` and `page_size` parameters to paginate through the content. Be consistent with the `page_size` value when paginating a specific job to ensure accurate page context.

3. **Input validation:** Validate your input parameters before sending a request to prevent issues with invalid input values and improve the overall stability of the service.

4. **Error handling:** Always check the `success` property in the response to verify if the request was successful. If an error occurs, handle it gracefully and use the provided error message to resolve the issue.

5. **Content handling:** Depending on your use case, you might need to process different content types such as HTML, XML, JSON, CSV, PDF, or images. Make sure to handle each content type appropriately and sanitize the content if necessary.

6. **The 'no_strip' parameter:** Use the `no_strip` parameter when the content structure and formatting are critical to the understanding of the information. Otherwise, rely on the default content processing to preserve context and improve readability for ChatGPT.

By adhering to these best practices and understanding the Web Requests plugin's capabilities, you can effectively use it to retrieve and process web content for your ChatGPT projects.

## Licensing

The Web Requests plugin is proprietary software developed by WeGPT.ai. Although the code is not publicly available on GitHub, this documentation is intended to provide information about the plugin's features and usage.

All rights to the code and documentation are reserved by WeGPT.ai. The code and documentation may not be redistributed, reproduced, or modified without prior written permission from WeGPT.ai.

For licensing inquiries or any other questions regarding the usage of the Web Requests plugin or its documentation, please contact josh@josholin.com for further information.
