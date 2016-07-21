# Technical SEO
Technical SEO refers to changes which need to be made by the developer or by more advanced functions of the CMS.

### Download
A new but extremely important ranking factor is now download speed or each page and a site as a whole.
There are many factors contributing to download speed, these have been addressed in the [front end speed](speed.md) docs.

### Robots.txt
The robots.txt file contains a series of directives that you wish the search engine robots/spiders to adhere to. It can be used to tell the search engine to ignore pages or directories. An incorrectly configured robots.txt file can stop whole sections of websites from being returned within the search results or in severe cases whole websites.  
[See more about the robots.txt here](http://www.robotstxt.org/)

### Sitemap.xml
A sitemap.xml file provides the search engine with a defined list of pages currently being hosted on your website. There are also other parameters which can be set that indicate to the search engine how you would like it to be crawled. If marked up correctly, the XML sitemap can also help to highlight to search engines which pages of the website are of the highest importance.

Search engines will typically look for an XML Sitemap before crawling a website. It should also be submitted to Google and Bing via their respective online Webmaster Tools.

### Canonical Link Element
The canonical link element can be used to highlight to the search engine which page should be seen as the dominant version. This is useful when the same content is accessible via multiple URLs.

### Redirects & Header Responses
Whenever a user visits your website, the server hosting it sends a header response code to the user's browser. This informs the user's browser how to interact with the webpage. It also can influence how the search engine interprets the page.

These will often go unnoticed by the general user.

#### Status: 200 OK
The "200 OK" header response is the most common of what we're covering. This informs the browser that all is working as it should be. However this should only be returned when the webpage is present and rendering.

#### Redirect: 301 Permanent
"301 Permanent" redirects indicate to search engine spiders that a page has permanently been moved to a new location. This means that the destination of the redirect will replace the source in search results, and that as much existing value as possible (PageRank, authority, backlinks, etc.) will be attributed to the destination.

When retiring old content we can redirect users to new but relevant content. This should be done using a "301 Permanent" redirect. Redirecting the user should be seamless for the user however be careful not to redirect the user to an irrelevant page as it may cause confusion.

#### Redirect: 302 Temporary
As with a "301 Redirect" the "302 Temporary" redirect will send the user to a new relevant page if the old page no longer exists. However, it's very important to note that none of the old page's authority and relevancy will be transferred. Therefore this type of redirect should only be used in special circumstances, i.e. when the redirect is actually *temporary*.  

#### Not Found: 404
This style of header known as the "404" is what the header response will return when the requested page or url no longer exists on the server. This is a bad practice to have 404 pages on your site. Pages that are removed should be "301" redirected to a relevant page instead of deleted.  
Using a "404" header on page responses that are actually found but perhaps not what you intended should respond with an alternative header (200, 403 "Forbidden" or other).

### Error Handling
When the user requests a page that is not present on the website (this could be if someone has linked to the website using an incorrect URL or they simply misspell the URL) they should be presented with an error page and the server should return a 404 header response. The 404 header response is an instruction to the search engine that it is not a valid page and therefore should not be indexed by the search engine (or if it has already been indexed it should be removed). It also helps to ensure that these errors are located easily using tools such as Google/Bing Webmaster Tools.

If a page cannot be found but is still returning a "200 OK" header response it can be deemed as a "Soft 404" (an error page which is functioning incorrectly) - this is an issue which needs to be avoided.
