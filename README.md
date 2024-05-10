# Distributed Web Crawler

A simple distributed web crawler built on the analytics engine engine and key-value store, designed for efficient and ethical web scraping. The crawler adheres to the robot exclusion protocol and limits request rates to individual web servers.

## Overview

The crawler:
- Uses Flame for distributed processing.
- Maintains a queue of URLs to visit using an RDD.
- Stores downloaded pages in a key-value store (KVS).
- Follows redirects and adheres to robots.txt for polite crawling.
- Extracts and normalizes links from HTML pages.

## Requirements

### Functionality
- **URL Queue**: Maintains a queue of URLs to visit, starting from a seed URL.
- **Politeness**: Adheres to robots.txt rules and enforces a delay between requests to the same server.
- **Page Storage**: Stores response details and HTML content in a persistent KVS table.
- **Link Extraction**: Extracts and normalizes links from `<a>` tags in HTML pages.

### Class and Arguments
- **Class Name**: `cis5550.jobs.Crawler`
- **Arguments**:
  1. Seed URL (required)
  2. Blacklist name (optional, ignore if not using EC2)

### Output
- **KVS Table**: `pt-crawl`
  - **Columns**:
    - `url`: Original URL
    - `responseCode`: HTTP response code
    - `contentType`: Content type (if provided)
    - `length`: Content length (if provided)
    - `page`: HTML content (if response code is 200 and content type is `text/html`)

### Robot Exclusion Protocol
- **robots.txt**: Fetch and parse robots.txt before other requests to a host.
- **Rules**:
  - `Allow: xxx`
  - `Disallow: xxx`
  - `Crawl-delay: yyy`
- **User-Agent**: `cis5550-crawler`
- **Request Delay**: Minimum 1-second delay between requests to the same server, unless overridden by `Crawl-delay`.

### URL Handling
- **Extraction**: Extract links from the `href` attribute of `<a>` tags.
- **Normalization**: Convert relative links to absolute URLs.
- **Filtering**: Ignore URLs with non-http/https protocols or ending in `.jpg`, `.jpeg`, `.gif`, `.png`, `.txt`.
- **Redirects**: Follow redirects (301, 302, 303, 307, 308) by adding new URLs to the crawl queue.
