# SEO for Developers

## Reference

<https://codefrontend.com/introduction-to-seo-for-react-developers/>
<https://www.youtube.com/watch?v=JSm4aQl4w_U>

## What is SEO

SEO is the process of increasing website traffic. General goal is that when someone clicks on a link of your site on SERP, they should engage with it as long as possible.

## SEO cateogories

SEO has two categories: on-page SEO and off-page SEO
- Off-page SEO: refers to the factors that we can not affect directly on the website, which is social media marketing, outsite link pointing to the website (Link liên kết), high-quality content.
- On-page SEO: refer to the factors that we can apply directly on our website. This is the developer's job.

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

### Sitemap

A file where you provide information about pages, videos and other files and the relationships between them

### robots.txt

- tell the search engine crawlers which URLs the crawlers can access on your site.
- this is used to overloading your site with requests

### Meta tags:

- Provide search engines with information about your website
- Are added to the `<head>` section
- Some meta tags
	`<meta name="description" content="">`: provide a short description of the page.
	`<meta name="viewport" content="">`: tells the browser how to the render the page on mobile device
	`<meta name="robots" content="">`:
	  - `content="noindex`: prevent search engine crawlers from indexing this page on your website
	  - `content="nofollow`: prevent search engine crawlers to crawl any of the links on the page

### Canonical URL

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

