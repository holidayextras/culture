# Organising code in backbone projects

## Entity types

We've extended the [Backbone library](http://backbonejs.org/)'s implementation of MVC with some of our own concepts - when adding new code it will tend to fit into one of these layers:

- **Routers** are initialised by the application's entry point, and map the URL you visit (including when the URL changes without a page reload) to a method on a controller.
- **Controllers** create and manage the high-level state of the application (which models, collections and views are currently in play). We use [dependency injection](https://github.com/holidayextras/culture/blob/master/clientside-javascript-best-practices.mkd#do-use-dependency-injection) to propagate this state further down.
- **Views** watch part of the user interface (usually a HTML document) for interactions, and communicate them back to models or controllers. They can render templates on the client, or augment HTML returned by the server.
- **Templates** can generate strings of HTML given simple data as a context. They contain minimal logic, and are preferably written in [handlebars](http://handlebarsjs.com/).
- **Presenters** decorate models with presentation logic. If it's too complicated for a template but not factual enough to be considered application state, [it probably belongs in a presenter](https://github.com/holidayextras/culture/blob/master/clientside-javascript-best-practices.mkd#do-transform-data-for-presentation-using-presenters).
- **Models** represent business entities (like Basket or User or Lounge), and contain the current state of the application.
- **Collections** are groups of related models, and functionality that applies to the group (eg filtering and sorting)
- **Factories** map simple data into instances of things. If you've got data representing a Product, and that Product might be a Hotel or a Carpark or a Lounge, feed the data to the factory and it'll give you a ready-to-use instance of the relevant subclass back.
- **Mixins/Concerns** are bundles of methods and properties that can be used to augment functionality into other objects. They can [help prevent](https://github.com/holidayextras/culture/blob/master/clientside-javascript-best-practices.mkd#do-choose-between-using-composition-and-inheritance-carefully) the inheritance graph getting too complicated.

## Entity relationships

While any module in the application _can_ require any other, sticking to certain conventions is preferred:

- (Anything that can "create" something in the list below can "require" it.)

-----

- Routers must require controllers.
- Controllers can create models, collections and views.
- Views _can_ create models and collections, but [it's preferable for controllers to](https://github.com/holidayextras/culture/blob/master/clientside-javascript-best-practices.mkd#do-use-controllers-to-set-up-instances).
- Views can require templates.

-----

- Presenters can be standalone, in which case views or other presenters may create them.
- ...or be associated with a model, and then that model may create them
- Collections can require models, but only to enable Backbone's behaviour to use them as a template when creating new instances.

-----

- Anything can require mixins.
- Anything can require libraries.
- Anything can require factories, but it's preferable for controllers to.
- Factories can require anything (but each factory only a specific type of entity)

-----

If you're adding a relationship between objects that isn't listed above, consider carefully whether it's a good idea, it's likely to be unusual enough to trip you or someone else up later.