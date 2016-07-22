# Schema / Rich snippets

_**Always check your schema markup before pushing live with the [online rich snippet testing tool](https://search.google.com/structured-data/testing-tool)**_

There are many different ways to implement rich snippets onto pages for Search engines like Google to index.  
* RDFa
* microdata
* JSON-LD

The one we have chosen to adopt is Microdata, although at some point moving to JSON-LD could be considered.  Google and Bing have expressed that they will be following the guidelines set out by [schema.org](http://schema.org).

### Reviews
There are two different types of review rich snippets, one for individual reviews and another for aggregated reviews.

A review is someone's evaluation of something. Google support reviews and ratings for a wide range of schema.org types, including businesses, products, and different creative works such as books or movies.

When Google finds valid reviews or ratings markup, we may show a rich snippet that includes stars and other summary info from reviews or ratings. In addition to the text of the review, a rating is an evaluation described on a numeric scale (such as 1 to 5).

#### Example
```html
<div itemscope="" itemtype="http://schema.org/Product">
  <span>Our <span itemprop="name">Gatwick airport parking</span> is rated
    <span itemprop="aggregateRating" itemscope="" itemtype="http://schema.org/AggregateRating">
      <meta itemprop="bestRating" content="100">
      <meta itemprop="worstRating" content="1">
      <span itemprop="ratingValue">90</span>% by
      <span itemprop="reviewCount">129354</span> guests
    </span>
  </span>
</div>
```


#### Guidelines
To be eligible for reviews and ratings rich snippets, be aware of the following extra guidelines:

* **Aggregate ratings:** An aggregate evaluation of an item by many people should be marked up as a schema.org/AggregateRating. Google may display aggregate ratings as rich snippets or, for certain types of items, answers in search results.
* **Refer clearly to a specific product or service.** Do this by nesting the review or ratings within the markup of another schema.org type, such as schema.org/Book or schema.org/LocalBusiness or by using that schema.org typed element as a value for the itemReviewed property.
* **Make sure the reviews and ratings you mark up are readily available to users from the marked-up page.** It should be immediately obvious to users that the page has review or ratings content.
* **Provide review and/or rating information about a specific item, not about a category or a list of items.** For example, “hotels in Madrid,” “summer dresses,” or “cake recipes” are not specific items.
* **No reviews are shown for adult-related products or services.**
* **Single reviewer name needs to be valid.** For example, "50% off until Saturday" is not a valid name for a reviewer.
* **Ratings that don't use a 5-point scale:**
By default, Google assumes that your site uses a 5-point scale, where 5 is the best possible rating and 1 is the worst, but you can use any other scale. If you do, you can mark up the best and worst ratings, and Google will scale that to the 5-star system used in rich snippets. The 10-point scale used above does just this. For further examples see below.
