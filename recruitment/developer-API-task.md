# Web Development API Task

## Your task is to create an API to manage a user persistence layer.

We would expect this task to take a few hours, however there is no strict time limit and you won't be judged on how long it took you to complete. Please find a few pointers below:

- Your solution must expose a user model and it's reasonable to expect that an individual user would have the following attributes:

  - **`id`** - _a unique user id_
  - **`email`** - _a user's email address_
  - **`givenName`** - _in the UK this is the user's first name_
  - **`familyName`** - _in the UK this is the user's last name_
  - **`created`** - _the date and time the user was added_

- It must have the ability to persist user information for at least the lifetime of the test.

- You API must expose functionality to create, read, update and delete (CRUD) models.

Although the main outcomes of the task are listed above, if you feel like you want to go that extra mile and show us what you're capable of, here is a list of potential enhancements that we have come up with:

- How your API is to be consumed (a custom interface or something like [Postman](https://www.getpostman.com/) or [Swagger](https://swagger.io/)).
- Use of an industry standard data exchange format.
- Sanitisation checks of inputs.
- Implementation of test coverage.

Alternatively if you can think of any other features that you feel would further enhance your API, then we'd love to see what you can come up with!

## ⚠ Important implementation note!

Your solution to this assessment _should_ be implemented using technologies with cross-platform support. We can't really think of any modern web technologies that aren't supported across all major operating systems (do let us know if you know about one). If you wish to implement your solution using .NET, please make sure you use **[.NET Core](https://www.microsoft.com/net)** which is supported on Linux, macOS and Windows.
