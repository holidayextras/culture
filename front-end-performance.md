# Front End Performance

## Overview
This is a collection of rules and guidelines aimed at speeding up front-end delivery of pages. We'll start based on Steve Souders principles from 'High Performance Websites', a book everyone should have on their shelves. If you got speed problems, I feel bad for you son...

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
Establishing connections is expensive. So are DNS Lookups. 40ms, 100ms, they add up. Make sure all scripts and styles are bundled into as few logical files as possible. This can be done at build time or on the fly by a plugin, like Previewable. 

Images are combined into sprites, or DataURIs. I don't like DataURIs any more, they cross the boundary of separating style and content and they invalidate caching. Change one style, all images have to be downloaded by the client. Cons outweigh the pros. Discuss. 

Interestingly HTTP/2 will change a lot of this. Although making superfluous HTTP requests will never be cool, combining elements, be they JS, CSS or images, is a hack to get around the limitations (intended behaviour at the time) of HTTP/1.1.

## Use a CDN
Put the assets closer to the user.

## Add an Expires header
One for the IT guys. The headers should be set, we should have a set strategy for images, css and js.

Policies
Change an image, change the filename.
Images, CSS & JS - 2 years
You are not meant to set them infinitely into the future, contravenes the specification.

We can only control the headers on our own files. Our tracking, marketing and monitoring beacons mean that over 70% of HTTP requests (on SEO pages) come from third parties, and they have to have short or even expired Expires headers, thats the way they work. I include HXTrack and Orion as 3rd parties, we just happen to write them.

## Gzip Components
Always. This doesn't just go for the HTML but for JS and CSS too. You would also prefer API responses to be zipped, we can only control our own, not those of third parties but it might be worth asking them politely if you see any that aren't. Goodwill, karma, etc.

If you ever see content that is not compressed, shout about it to the Ops guys, fellow devs, find out if the file is in the wrong directory, get it moved or get the compression applied. Less bytes through the pipe, got to be a good thing.

Do not attempt to zip images. This should be obvious, but zip and gif/png use run-length encoding, zipping a jpeg might actually increase size and it'll just be extra work for zero benefit.

## Put stylesheets at the top
The browser cannot render your page without the styles. Download them in the head, as an external file, not inline. And it is best to use link in favour of @import. The latter causes the blank white screen problem. Even if the @import directive is in the head, browsers don't observe that order and the stylesheet is downloaded last.

If certain styles are not used until later, it is fine to defer them. But that should not lead to Flash Of Unstyled Content, ever! Deferring a stylesheet just to get a better score on some index is not the point. It's all about the user experience, and unstyled or partly styled content makes for a poor user experience. Break the styles into logical blocks, one file to be downloaded for initial render, another to be downloaded post-load.

Put stylesheets in the document HEAD, with the LINK tag.

## Put javascript at the bottom
Javascript blocks rendering. Why would the browser do all the work to construct the Render Tree, layout and then paint the page when we can change the DOM and styles via javascript? 

http://www.webpagetest.org/video/compare.php?tests=150817_JR_YP3,150817_J5_YNJ

If anyone doubts the worth of this, have a look at the experiment above. The difference is staggering, and particularly important where jQuery is involved

We have a difficult situation on the SEO pages. Optimizely requires jQuery. On pages where Optimizely is choosing a variant and redirecting, it must be in the HEAD and as early in the page as possible. 

## Optimise images
'Optimise' - what do we mean by that word? Anyone responsible for creating and uploading images to the site . There's a valuable webinar by Guy Podjarny, I'll dig out the link. TODO
We have many images that are the wrong format. We have photographs saved as pngs, and logos saved as jpegs. This is topsy turvey and implies that there is help needed on understanding what the different types of compression do and how they should be applied. 

Post-loading images doesn't absolve us from our responsibility to compress images. Render and TripApp hotel availability for Gatwick currently has the user download just short of 2MB of images. That is too much. Let's decide a file size budget and stick to it. I propose 1MB for the whole page.

## Make fewer HTTP requests - file combining strategies
As with all rules, there are caveats here. Casper on 'Poker Night Live' always said - "No rules, just tools". Our current strategy is to bundle everything up on a given page so that when the user comes back to that exact page, they have the file. But there is no concept of sitewide caching, the combined css and js files on foo.html can be different to bar.html if just one extra function or file is included. Some amount of redundancy could be permitted to allow reuse of files across pages, rather than a dedicated combined for every page. 

Some files change frequently, others (1st and 3rd party frameworks & libraries like jQuery) barely at all. If I change an alert message, the user has to download the whole file, not just the changed stuff, including jQuery and the whole slew of unchanged libraries.

Even further, different devices make use of different functions. Could it be feasible to conditionally include files common.js, desktop.js, mobile-tablet.js?

