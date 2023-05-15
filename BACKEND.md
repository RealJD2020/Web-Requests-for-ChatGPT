# Web Requests - ChatGPT's Web Browser

The "Web Requests" module is a web browser implemented within ChatGPT that allows you to perform various web-related tasks such as scraping web content, parsing data, and executing HTTP requests. It is built using the Quart framework, which is an asynchronous web framework for Python.

## Features

The "Web Requests" module provides the following features:

1. **Scraping URL**: You can scrape the content of a web page by providing the URL. The module retrieves the HTML content of the page, parses it, and returns the text content. It also supports pagination, allowing you to retrieve content in chunks.

2. **Parsing Job**: You can parse the content of a previously scraped URL by providing the job ID. This feature is useful when you want to extract specific information from the scraped content using regular expressions (regex). You can define a regex pattern and the module will return the matched results.

3. **Google Search**: You can perform a Google search by providing a search query. The module uses the Google Custom Search JSON API to retrieve search results. You can specify the number of results to scrape and whether to follow the links in the search results.

4. **Caching**: The module implements an in-memory cache to store previously scraped content and parsed results. This helps in reducing the load on external resources and improves response times for repeated requests. The cache is automatically cleaned every 15 minutes.

5. **Error Handling**: The module handles various types of errors, including invalid URLs, invalid input parameters, network errors, and unsupported content types. It provides meaningful error messages to assist in troubleshooting.

6. **Command Line Interface**: The module includes a command line interface (CLI) that allows you to interact with the browser, view statistics, purge the cache, restart the server, and shut down the server.

## Implementation Details

The "Web Requests" module is implemented in Python using the Quart framework, which is a lightweight asynchronous web framework. It uses the `httpx` library for making HTTP requests, `BeautifulSoup` for parsing HTML content, `defusedxml.ElementTree` for parsing XML content, `pdfminer` for extracting text from PDFs, and other libraries for handling various content types.

The module uses asynchronous programming to handle multiple concurrent requests efficiently. It leverages asyncio, which is a library for writing single-threaded concurrent code using coroutines, multiplexing I/O access over sockets and other resources, running network clients and servers, and other related primitives.

The module also includes an in-memory cache for storing previously scraped content and parsed results. The cache is implemented as a dictionary and is protected by locks to ensure thread safety. The cache is periodically cleaned to remove stale entries.

## Usage

To use the "Web Requests" module, you need to send HTTP requests to the appropriate endpoints exposed by the server. The following endpoints are available:

1. **Scrape URL**: Endpoint: `/scrape_url` (POST)
   - Parameters:
     - `url`: The URL of the web page to scrape.
     - `page` (optional): The page number for pagination (default: 1).
     - `page_size` (optional): The number of items per page for pagination (default: 10000).
     - `is_search` (optional): Set to `true` if the URL is a search query (default: `false`).
     - `follow_links` (optional): Set to `true` if you want to follow links in search results (default: `false`).
     - `num_results_to_scrape` (optional): The number of search results to scrape (default: 3).
     - `job_id` (optional): A unique ID for the job (default : generated using UUID).
     - `refresh_cache` (optional): Set to `true` to bypass the cache and fetch the content again (default: `false`).
     - `no_strip` (optional): Set to `true` to preserve HTML tags and formatting in the scraped content (default: `false`).

   This endpoint scrapes the content of a web page specified by the URL. It returns the scraped content as text. If the URL is a search query, it performs a Google search and returns the search results.

2. **Parse Job**: Endpoint: `/parse_job` (POST)
   - Parameters:
     - `job_id`: The ID of the job whose content you want to parse.
     - `url`: The URL of the web page that was previously scraped.
     - `regex` (optional): The regex pattern to match against the scraped content.
     - `page` (optional): The page number for pagination (default: 1).
     - `page_size` (optional): The number of items per page for pagination (default: 10000).
     - `parse_id` (optional): A unique ID for the parsing job (default: generated using UUID).
     - `refresh_cache` (optional): Set to `true` to bypass the cache and fetch the content again (default: `false`).

   This endpoint parses the content of a previously scraped job using a regex pattern. It returns the matched results.

3. **Well-Known Endpoint**: Endpoint: `/.well-known/ai-plugin.json` (GET)

   This endpoint provides the AI plugin metadata in JSON format.

4. **OpenAPI Specification**: Endpoint: `/openapi.json` (GET)

   This endpoint provides the OpenAPI specification for the "Web Requests" module.

5. **Logo**: Endpoint: `/logo.png` (GET)

   This endpoint returns the logo image for the "Web Requests" module.

## Environment Variables

The "Web Requests" module uses the following environment variables:

- `API_KEY`: Your API key for the Google Custom Search JSON API.
- `CX`: Your custom search engine ID for the Google Custom Search JSON API.
- `OPENAI_API_KEY`: Your API key for the OpenAI GPT-3 API.

Make sure to set these environment variables before running the server.

## Command Line Interface

The "Web Requests" module includes a command line interface (CLI) that allows you to interact with the browser. The following commands are available:

- `stats`: Displays statistics such as requests per hour and cache information.
- `purge cache`: Clears the cache.
- `purge parsed cache`: Clears the parsed cache.
- `restart`: Restarts the server.
- `quit`: Shuts down the server.

You can enter these commands in the terminal to perform the corresponding actions.

## Conclusion

The "Web Requests" module provides a powerful web browsing capability within ChatGPT. It allows you to scrape web content, parse data, perform Google searches, and execute HTTP requests. The module is designed to be efficient, scalable, and user-friendly, providing a seamless web browsing experience within the ChatGPT environment.
