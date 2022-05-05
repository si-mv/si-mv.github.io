### Warm up
The factorial function takes a positive integer and multiplies it by all the positive integers smaller than it.
For example
$$
\begin{aligned}
5!
& = 5 \times 4 \times 3 \times 2 \times 1 \\
& = 120
\end{aligned}
$$
Using JavaScript, write a function
```js
factorial(n)
```
which computes the factorial of a given positive integer.

### Solution
```js
// Using recursion
function factorial (n) {
    return n == 1 ? 1 : n * factorial(n - 1)
}
// Using a loop
function factorial (n) {
    let ans = 1
    for (let i = 1; i < n+1; i++) {
        ans *= i
    }
    return ans
}
```


# REST APIs

## REST
- Means Representational State Transfer
- It is an *architectural style* which uses http requests to access and use data

## API
- Means Application Programming Interface
- It is a way of using an app with code, not your finger


## Methods
There are $4$ main REST API methods

## GET
Get the requested REST API resource.
E.g.
```http
GET https://myapi.com/api/films/109jr3r2
```

## POST
Create a new resource.
E.g.
```http
POST https://myapi.com/api/films
Content-Type: application/json
{
    "id": "mt4bwu",
    "title": "Star Wars",
    "year": "1978"
}
```

## PUT
Edit the resource.
E.g.
```http
PUT https://myapi.com/api/films/mt4bwu
Content-Type: application/json
Authorization: Bearer 38j04923mcht9n23098h32k9m8j4c20398nhcm09h
{
    "year": "1977"
}
```

## DELETE
Delete the resource.
E.g.
```http
DELETE https://myapi.com/api/films/mt4bwu
Authorization: Basic k239mu20m0fm90
```


## Endpoints
What do you think the following requests do?

```http
GET http://mycoolapp.com/api/users
```

```http
DELETE http://mycoolapp.com/api/films/298m23ff
```

```http
POST http://mycoolapp.com/api/characters
Content-Type: application/json
{
    "name": "Han Solo",
    "occupation": "Pilot"
}
```

```http
PUT http://mycoolapp.com/api/users?firstName=Simon
Content-Type: application/json
{
    "description": "is awesome"
}
```


## Response codes
How many http response codes do you know?
<textarea style="width:700px;height:300px;font-size:1em;background-color:dimgrey;color:white;" />

## 200
The operation has succeeded

## 201
The resource was successfully created

## 400
The client sent an invalid request (e.g. missing username)

## 401
The client failed to authenticate with the server (e.g. wrong password)

## 403
The user does not have permission to do this action

## 404
The requested resource could not be found

## 500
Internal server error

## 503
Service is unavailable


## Best practices

### Accept and respond with JSON

### Use nouns instead of verbs for endpoints
Bad
```http
http://myapp.com/getUser/123
```
Good
```http
https://myapp.com/users/123
```

### Handle errors gracefully and return standard error codes

### Allow filtering, sorting and pagination
```http
myapp.com/users/?sort=firstName?dir=asc?cursor=14?limit=5
```

### Maintain good security practices

### Cache data to improve performance

### Version your API


## Tell me something about REST APIs


## Assignment :Bleeter 🐑
We will create Bleeter - an app where users can post messages up to 140 characters in length!
