# Basic Auth


## Authorization headers

There are different types of authorization headers we can send. The easiest one is `BASIC`. We take the username and password, and we encode the string
```js
`${username}:${password}`
```
in base 64 to generate a token.

In the browser, you can encode with
```js
btoa('nimonian:Potato23')
```
and decode with
```js
atob('bmltb25pYW46UG90YXRvMjM=')
```

We then send the token like this:
```http
GET https://myapp.com/api/secrets/4351
Authorization: BASIC bmltb25pYW46UG90YXRvMjM=
```

And we use
```js
const auth = require('basic-auth')
```
like this
```js
const user = auth(req)
```
on the server to decode the token.


## Hashing

A hashing function takes data and turns it into a hash using an algorithm. E.g.
```js
digitalRoot(6109) // 7
```

Every hash has the same length, and it looks absolutely nothing like the input.

If you're given the hash, you can't get the data back.

If the data changes even a tiny bit, the hash changes completely.


## Salting

We want to hash passwords before storing them in the database, because hashes are useluess to hackers.

When a user sends us their password, we hash it, and check it against the database's hashed password to authenticate the user.

But hashing the password directly isn't a good idea. So many people have the password "password" that hackers could look for the most common hash in the databse and guess what the underlying password is.

A salt is a random string of characters we add to the password before hashing to enforce uniqueness:
```js
database.hashedPassword = hashfunction(password + salt)
```


## Improve Bleeter 🐑
We will implement hashing, salting and basic-auth in order to improve our Bleeter app's security.
