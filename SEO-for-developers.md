# SEO for Developers

## Terms

- Sitemap: a file where you provide information about pages, videos and other files and the relationships between them
- robots.txt:
  - tell the search engine crawlers which URLs the crawlers can access on your site.
  - this is used to overloading your site with requests
- Meta tags:
  - Provide search engines with information about your website
  - Are added to the `<head>` section
  - Some meta tags
  	`<meta name="description" content="">`: provide a short description of the page.
  	`<meta name="viewport" content="">`: tells the browser how to the render the page on mobile device
  	`<meta name="robots" content="">`:
  	  - `content="noindex`: prevent search engine crawlers from indexing this page on your website
  	  - `content="nofollow`: prevent search engine crawlers to crawl any of the links on the page
- Duplicate URLs: if you have single page that is accessable by multiple URLs, or different pages with similar content, Google sees these as duplicate versions of the same page. Google will choose one URL as the canonical version and crawl that, others are considered duplicate URLs and crawled less.
  - Why have duplicated URLs
    - Support multiple device types
    ```html
    https://example.com/news/koala-rampage
	https://m.example.com/news/koala-rampage
	https://amp.example.com/news/koala-rampage
    ```
  - Specify canonical URL for duplicate URLs
  ```html
  <link rel="canonical" href="https://example.com/dresses/green-dresses" />
  ```