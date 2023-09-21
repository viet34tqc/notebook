# SEO for Developers

## Reference

<https://dev.to/this-is-learning/top-20-must-know-tips-for-web-accessibility-452>
<https://codefrontend.com/introduction-to-seo-for-react-developers/>
<https://www.youtube.com/watch?v=JSm4aQl4w_U>

## What is SEO

SEO is the process of increasing website traffic. General goal is that when someone clicks on a link of your site on SERP, they should engage with it as long as possible.

## SEO cateogories

SEO has two categories: on-page SEO and off-page SEO
- Off-page SEO: refers to the factors that we can not affect directly on the website, which is social media marketing, outsite link pointing to the website (Link liên kết).
- On-page SEO: refer to the factors that we can apply directly on our website:
	+ Title and Description tags
  + Meta tags
  + Keywords
  + Content of page
  + Heading tags
  + Anchor text
  + Image alt text
  + Internal links
  + External links
- Technical SEO: refers to improving the technical aspects of a website in order to increase the ranking of its pages in the search engines. The main goal of Technical SEO is to ensure that search engine crawlers can crawl and index a website without any problems. Making a website faster, easier to crawl and understandable for search engines are the pillars of technical optimization
  - Specify a Preferred Domain
  - Optimize Robots.txt
  - XML Sitemap Optimization
  - Use SSL and HTTPS
  - Optimize Your URL Structure
  - Navigation and Site Structure
  - Implement Structured Data Markup
  - Canonical URLs
  - Optimize 404 Page
  - Ensure your site is Mobile-friendly
  - Improve Your Site Speed
  - Find and Fix Crawl Errors
  - Fix Broken Internal and Outbound Links
  - Check the Page Depth of Your Site
  - Check Temporary 302 Redirects
  - Set up Correctly the 301 Redirects After Site Migration
  - Pagination and Multilingual Websites
  - Register your site with Webmaster tools

## SEO metrics

Below are some metrics of SEO that reflects your SEO score.

### Click-through rate (CTR)

This defines how likely a user click on your link displayed on SERP. The higher CTR the better, that means you have a relevent title and description.

### Bounce rate

If a user click on your site and click back immediately, that's a bounce. The higher bounce rate is, the less likely your site is to rank well.

### Dwell Time

If a user stay on the page, Google will keep track of the dwell time, which is the amount of time they spend there before clicking back to SERP. The longer the better

### Outbound Link

It signals what your content is about. It's really good for your SEO if you use your outbound links to other really good sites that are related to the content of your page.

## On-page techniques

- Using `title` tag: `title` tag should contain keyword.
- Using `meta` description: Meta description should contain keyword.
- Using `meta` keywords: relevent keywords to the page
- Using favicon
- Apply `alt` text for images
- Optimize speed
  - Optimize images
  - Minify and compress assets
- URL: needs to explain what the website is about, easy to understand. The shorter the better.
- Internal links: Link to 2-5 other pages on your site
- External links: Link to 2-5 trusted websites

## Technical SEO techniques


- Implement Structured Data Markup: 
	+ Schema.org takes care of all the structured data needs on your website.
	+ Search engines like Google can pick up this data and present your page in an enhanced way if you use it to describe products, reviews, events, and recipes 
	+ Use Structured Data Testing Tool (https://search.google.com/structured-data/testing-tool) tool to check
- Canonical URLs:
	+ Follow guideline https://developers.google.com/search/docs/advanced/crawling/consolidate-duplicate-urls
- Optimize 404 Page:
	+ Follow guideline https://developers.google.com/search/docs/advanced/crawling/custom-404-pages
- Make Sure Your Website is Mobile-Friendly:
	+ Use Mobile-Friendly Test (https://search.google.com/test/mobile-friendly) to check
- Improve Your Site Speed
	+ You need to make sure your site loads quickly. The ideal website load time is 3 seconds or less.
	+ Use PageSpeed Insights (https://pagespeed.web.dev/) tool to check
- Fix Duplicate Content Issues
	+ Duplicate content is exact or near-duplicate content that appears on the web in more than one place.
- Fix Crawl Errors:
	+ A list of crawl errors can be found in your Search Console, and you should fix them
- Fix Redirect Chains and Loops
	+ Redirects should go from page A to page B
- Fix Broken Internal and Outbound Links
	+ A list of broken internal and outbound links can be found in your Site Audit report, and you should fix them
- Fix HTTP Links on HTTPS Pages
	+ Need to update all http:// to https:// or //
- Check the Page Depth of Your Site
	+ Ideally, pages shouldn't be any further into your site than 3 clicks deep
- Check Temporary 302 Redirects
	+ A 302 redirect lets search engines know that a website or page has been moved temporarily
	+ You would use this type of redirect if you want to send users to a new site or page for a short period of time
- Set up Correctly the 301 Redirects After Site Migration
	+ Set up the redirect codes: Set up server-side redirects from your old URLs to the new ones
	+ Minimize the number of 301 redirects for your site
- Pagination and Multilingual Websites
	+ Pagination with rel="next" and rel="prev"
	+ Language and location targeting through Hreflang
- Set up Google Search Console
	+ To monitor websites
- Set Up Google Analytics
	+ To monitor, analyze and optimize user behaviors on websites

## SEO Audit

- Use Lighthouse tool @ https://web.dev/lighthouse-seo/
- Technical SEO Audit https://www.semrush.com/blog/technical-seo-audit/ 
- Site Audit Issues https://www.semrush.com/kb/542-site-audit-issues-list 

## Rules

- Create good content
- Using sematic HTML: `article`, `heading`, `img` with `alt` text, `title`
- Using `meta` tag
- Using Schema for your article, review... Google can read your schema data from the page and format it properly on SERP.
- Optimize speed
- Always use "noopener" or "noreferrer" for links opened in new tabs
`noopener` prevents the new site from the links use `window.opener.location` to redirect the user to a fake website.
`noreferrer` not only fix the issue that `noopener` do but also prevent `Referrer` header from being sent to the new tab.

## Some other terms

## Sitemap

A file where you provide information about pages, videos and other files and the relationships between them

## robots.txt

- tell the search engine crawlers which URLs the crawlers can access on your site.
- this is used to overloading your site with requests

## Type of links

Links on our website:

- Interal links: links that navigate you to another page on your website. These links allow viewers to stay on your page longer as they are redirected around your website. As a result, it increases your website authority which contributes to ranking.
- External links (Outbound links): links that navigate you to another domain.
	+ nofollow links: link with `rel="nofollow`. This tells Google not to trust this link and to discount it from consideration => it shouldn't help the target link to rank any better. Why use this: use when you don't control the links that are added to your page

Links on other websites:

- Backlink (Inbound links): links created on other website and point to our website. These backlinks will contribute to your ranking on the SERP. The more backlinks a website receives, the more relevant, useful and important it is.

## Meta tags:

- Provide search engines with information about your website
- Are added to the `<head>` section
- Some meta tags
	`<meta name="description" content="">`: provide a short description of the page.
	`<meta name="viewport" content="">`: tells the browser how to the render the page on mobile device
	`<meta name="robots" content="">`:
	  - `content="noindex`: prevent search engine crawlers from indexing this page on your website
	  - `content="nofollow`: prevent search engine crawlers to crawl any of the links on the page

## Canonical URL

If you have single page that is accessable by multiple URLs, or different pages with similar content, Google sees these as duplicate versions of the same page. Google will choose one URL as the canonical version and crawl that, others are considered duplicate URLs and crawled less.
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

## SEO Tool

- Broken Link Checker: https://www.brokenlinkcheck.com, https://ahrefs.com/broken-link-checker
- Dead Link Checker: https://www.deadlinkchecker.com
- Backlink Checker: https://ahrefs.com/backlink-checker
- XML Sitemap Validator: https://www.xml-sitemaps.com/validate-xml-sitemap.html
- Keyword Rank Checker: https://ahrefs.com/keyword-rank-checker
- Free Keyword Generator: https://ahrefs.com/keyword-generator
- Website “Authority” Checker: https://ahrefs.com/website-authority-checker
- robots.txt Validator and Testing Tool: https://technicalseo.com/tools/robots-txt
- Schema Markup Generator (JSON-LD): https://technicalseo.com/tools/schema-markup-generator
- Rich Results Test: https://search.google.com/test/rich-results
- SEO Browser: https://www.seo-browser.com
- Mobile-Friendly Test: https://search.google.com/test/mobile-friendly
- SERP Snippet Optimization Tool: https://www.highervisibility.com/seo/tools/serp-snippet-optimizer
- The hreflang Tags Generator Tool: https://www.aleydasolis.com/english/international-seo-tools/hreflang-tags-generator
- Map Broker XML Sitemap Validator: https://ipullrank.com/tools/map-broker/index.php
