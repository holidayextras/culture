# Front End Performance

## Overview
This is a collection of rules and guidelines aimed at speeding up front-end delivery of pages. We'll start based on Steve Souders principles from 'High Performance Websites', a book anyone touching websites should have on their shelves.

## Basic guidelines
 * Make fewer HTTP requests
 * Use a CDN
 * Add an Expires header
 * Gzip components
 * Put stylesheets at the top
 * Put javascript at the bottom
 * Avoid CSS expressions
 * Make javascript and stylesheets external
 * Reduce DNS lookups
 * Minify javascript
 * Avoid redirects
 * Remove duplicate scripts
 * Configure (or LOSE) ETags
 * Make AJAX cacheable???
 
 * Optimise images 

## Make fewer HTTP requests
Establishing connections is expensive. So are DNS lookups. So do it less often, combine files wherever possible. Make sure all scripts and styles are bundled into as few logical files as possible. This can be done at build time or on the fly by a plugin, like Previewable. 

Small images should be combined into sprites. I don't like DataURIs any more, they cross the boundary of separating style and content and they invalidate caching. Change one style, all images have to be downloaded again by the client.

HTTP/2 will change a lot of this. But that's for another (fast approaching) day.

\* Further notes on this and combining strategy at the foot of page.

## Use a CDN
Put the assets closer to the user. HTML, CSS, JS. We have a number of CDNs that we use. TripApp uses Cloudfront for their js & css assets, and even front static1 on cloudfront. The SEO and PPC pages use static1 and static4.holidayextras.com, which are both on Cloudflare. These also have the benefit of not including the sizeable HX cookies in every request, where they are completely irrelevant.

Whatever system you are on, the important thing is to be consistent. Pick a CDN and use it for everything. More chance of reusing connections. Domain sharding is an anti-pattern now, and with HTTP/2 will actually be detrimental to performance.

There are pages currently with assets called from www., secure., static1, static4 and cloudfront. Stop this. And if you see it, fix it.

## Add an Expires header
One for the IT guys as well. The headers should already be set, we should have a set strategy for images, css and js.

Policies:

* Change an image, change the filename.
* Images, CSS & JS - 2 years

You are not meant to set Expires headers into infinity, contravenes the specification.

We can only control the headers on our own files. Our tracking, marketing and monitoring beacons mean that over 70% of HTTP requests (on SEO pages) come from third parties, and they have to have short or even expired Expires headers, thats the way they work. I include HXTrack and Orion as 3rd parties, we just happen to write them.

## Gzip Components
Always. This doesn't just go for the HTML but for JS and CSS too. You would also prefer API responses to be zipped, we can only control our own, not those of third parties but it might be worth asking them politely if you see any that aren't. Goodwill, karma, etc.

If you ever see content that is not compressed, shout about it to the Ops guys, fellow devs, find out if the file is in the wrong directory, get it moved or get the compression applied. Less bytes through the pipe, got to be a good thing.

Do not attempt to zip images. This should be obvious, but you never know.

## Put stylesheets at the top
The browser cannot render the page without the styles. Download them in the HEAD, as an external file, not inline. 

If certain styles are not used until later, it is fine to defer them, post load them with javascript. But that should not lead to Flash Of Unstyled Content! Deferring a stylesheet just to get a better score on some index is not the point. It's all about the user experience, and unstyled or partly styled content makes for a poor one. Break the styles into logical blocks, one file to be downloaded for initial render, another to be downloaded post-load.

Put stylesheets in the document HEAD, with the LINK tag.

## Put javascript at the bottom
Javascript blocks rendering. Why would the browser do all that work straight away when we can change the DOM and styles via javascript? 

http://www.webpagetest.org/video/compare.php?tests=150817_JR_YP3,150817_J5_YNJ

If anyone doubts the worth of this, have a look at the experiment above. The difference is staggering, and particularly important where jQuery is involved. Not deferring anything here, this is simply a case of moving the javascript request to the foot of the page.

We have this situation on the SEO pages. Optimizely requires jQuery. On pages where Optimizely is choosing a variant and redirecting, it must be in the HEAD and as early in the page as possible. Otherwise the initial page is rendered, and then the screen goes blank once you are redirected. But on a variant, where the page is actually going to be loaded, we should shift Optimizely and jQuery to the foot of the page to prevent the Optimizely js from blocking.

## Reduce DNS lookups
Some pages I've seen have requests to www., secure., static4 and static1. All the files are available via staticx. So pick one and use it. Domain sharding is no longer a best practice, so just pick either static1 or static4 and call it consistently throughout the page. If you see a piece of content being called from secure, change it, please. Third party tags are a necessary evil which we can't totally control, but we can at least control our own requests.

## Minify javascript
Should do, but we don't. I don't know if that is because we have had it cause errors before. Would have to be careful that it was all done in the correct order, when combining files etc. No need to obfuscate, that only gives tiny improvement comparatively speaking not worth the effort or potential risk.

## Avoid redirects
We do this apart from when split testing. A necessary evil unless someone wants to make a name for themselves by writing server-side optimizely. Seriously, hack-day anyone? Easy for non-tech to change, but without the pain of going through redirects, half loading the page etc.

## Remove duplicate scripts
## Configure (or LOSE) ETags
We've turned these off on HXML & SEO. I've seen them on TripApp. If you're serving from >1 server, and if the inode is involved in the creation of the ETag, then they just want to be turned off otherwise they'll just unnecessarily invalidate a users cache of a resource.

## Make AJAX cacheable???

## Optimise images
'Optimise' - what do we mean by that? There's a great webinar/video by Guy Podjarny, http://www.oreilly.com/pub/e/3122?imm_mid=0bdf5b&cmp=em-na-webcast-info-webcast_20140616&sidebar-link 

We have many images that are in the wrong format. Save photographs as jpgs, and logos as pngs.

Post-loading images doesn't absolve us from our responsibility to compress images. Render and TripApp hotel availability for Gatwick currently has the user download just short of 2MB of images. That is too much. Let's decide a file size budget and stick to it. I propose 1MB for the whole page as a starter. A year or two ago a leading supermarket committed to <400kb as a total page weight budget. Now the average web page has 

## Make fewer HTTP requests - file combining strategies
As with all rules, there are caveats, you shouldn't just follow it blindly. "No rules, just tools". Our current strategy is to bundle everything up on a given page so that when the user comes back to that exact page, they have the file. But there is no concept of sitewide caching, the combined css and js files on foo.html can be different to bar.html if just one extra function or file is included. Some amount of redundancy could be permitted to allow reuse of files across pages, rather than a dedicated combined for every page. On the other hand, we have a situation in some SEO pages where >70% of styles are unused.

Some files change frequently, others (1st and 3rd party frameworks & libraries like jQuery) barely at all. If I change an alert message, the user has to download the whole file, not just the changed stuff, including jQuery and the whole slew of unchanged libraries.

Different categories of devices make use of different functions and styles. Would it be feasible to conditionally include files common.js, desktop.js, mobile-tablet.js?

