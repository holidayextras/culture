# Web Development API Task

## Your task is to create an API to manage a user persistence layer.

We would expect this task to take a few hours, however there is no strict time limit and you won't be judged on how long it took you to complete. Please find a few pointers below:

Your solution _must_:

  - Expose a **user** model. It's reasonable to expect that a user has (at least) the following attributes:
    - **`id`** - _a unique user id_
    - **`email`** - _a user's email address_
    - **`givenName`** - _in the UK this is the user's first name_
    - **`familyName`** - _in the UK this is the user's last name_
    - **`created`** - _the date and time the user was added_
  - Have the ability to persist user information for at least the lifetime of the test.
  - Expose functionality to create, read, update and delete (CRUD) users.
  - Be easily consumable by a plain HTTP client (e.g. cURL or Postman) or, if using a transport other than HTTP, be trivial to write a client application to consume it.

Although the main outcomes of the task are listed above, if you feel like you want to go that extra mile and show us what you're capable of, here is a list of potential enhancements that we have come up with:

- Provide a way for your API to be easily tested/consumed (e.g. a custom front-end, a [Postman](https://www.getpostman.com/) collection or a [Swagger/OpenAPI](https://swagger.io/) API specification).
- Use of an industry standard data exchange format.
- Sanitisation checks of inputs.
- Implementation of test coverage.

Alternatively if you can think of any other features that you feel would further enhance your API, then we'd love to see what you can come up with!

Since we care more about having a holistic picture of your software engineering skills than your proficiency in particular technologies or programming languages, you are free to use the tools and technologies you feel most confident with and that will present your work in the best possible light (while taking into account the caveats in the sections below). Node.js is our technology of choice for back end development and implementing your assessment using Node.js will be a point in your favour. However, be sure to weigh that choice against the fact that, if you're a successful candidate past this stage, you will be expected to demonstrate in person your proficiency with the tools and technologies you chose to implement this assessment. As we mentioned initially, what matters most is how well rounded a software engineer you are, since any tools, technologies and programming languages can always be learned.

## ⚠ Important implementation note!

Your solution to this assessment _should_ be implemented using technologies with cross-platform support. We can't really think of any modern web technologies that aren't supported across all major operating systems (do let us know if you know about one).

### .NET

 _(Please ignore this section if your solution isn't implemented using .NET.)_

If you wish to implement your solution using .NET, please make sure you use **[.NET Core](https://www.microsoft.com/net)** which is supported on Linux, macOS and Windows. Your solution is expected to **build** with the .NET Core SDK by simply running `dotnet build`. If more complex steps are required to build your solution, you are expected to provide _complete and detailed_ instructions for a successful build.
