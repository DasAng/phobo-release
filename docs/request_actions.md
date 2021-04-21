# Request actions

- [Http request](#http-request)
- [Http request with body](#http-request-with-body)
- [Http status](#http-status)
- [Http ok](#http-ok)


---

## Http request

This action will perform a HTTP/HTTPS request to the specified url.

`Regex`:

```shell
/http (get|head|options|delete|post|put|patch) `([^`]+)`\s*(?=(?:.*headers=`(?<headers>[^`]+)`)?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
http <method> `<url>` [headers=`<headers>`]
```

- **`method`**: this parameter is required and specifies the request method. It can be one of the following:
    - get
    - head
    - options
    - delete
    - post
    - put
    - patch
- **`url`**: this parameter is required and is url to retrieve data. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter
- **`headers`**: this parameter is optional. Specify the headers to use with the request. The format for specifying headers must be a comma separated name value pair like so:
    
    \<name>:\<value>

    For example:

    > headers=\`content-type:application/json,content-length:1024`

The result of the action is a JSON value of the following form:

```json
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the HTTP headers that the server responded with
  // All header names are lower cased and can be accessed using the bracket notation.
  // Example: `response.headers['content-type']`
  headers: {},
}
```

Example of usage:

- Make a HTTP get request for https://myapi.com/users/ 

    > http get \`https://myapi.com/users` 

- Make a HTTP get request for https://myapi.com/users/ and set headers

    > http get \`https://myapi.com/users\` headers=\`content-type:application/json,content-length:1024\`

- Make a HTTP head request for https://myapi.com/users/ 

    > http head \`https://myapi.com/users`

- Make a HTTP delete request for https://myapi.com/users/ 

    > http delete \`https://myapi.com/users`

- Make a HTTP options request for https://myapi.com/users/ 

    > http options \`https://myapi.com/users`

- Make a HTTP post request for https://myapi.com/users/ 

    > http options \`https://myapi.com/users`

- Make a HTTP put request for https://myapi.com/users/ 

    > http put \`https://myapi.com/users`

- Make a HTTP patch request for https://myapi.com/users/ 

    > http patch \`https://myapi.com/users`

## Http request with body

This action will perform a HTTP/HTTPS request to the specified url. You can specify a JSON data for the request body.

`Regex`:

```shell
/http with body (get|head|options|delete|post|put|patch) `([^`]+)`\s*(?=(?:.*headers=`(?<headers>[^`]+)`)?)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
http with body <method> `<url>` [headers=`<headers>`]
"""
<body>
"""
```

- **`method`**: this parameter is required and specifies the request method. It can be one of the following:
    - get
    - head
    - options
    - delete
    - post
    - put
    - patch
- **`url`**: this parameter is required and is url to retrieve data. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter
- **`headers`**: this parameter is optional. Specify the headers to use with the request. The format for specifying headers must be a comma separated name value pair like so:
    
    \<name>:\<value>

    For example:

    > headers=\`content-type:application/json,content-length:1024`
- **`body`**: this parameter is required and is specified in a docstring. The JSON data to send in the body of the request. You can use an [Intrinsic expression](intrinsic_expression.md) for this parameter

The result of the action is a JSON value of the following form:

```json
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the HTTP headers that the server responded with
  // All header names are lower cased and can be accessed using the bracket notation.
  // Example: `response.headers['content-type']`
  headers: {},
}
```

Example of usage:

- Make a HTTP post request for https://myapi.com/users/ with body *{"age": 10}*

    > http with body post \`https://myapi.com/users/\`  
    > """  
    > {"age": 10}  
    > """

## Http status

This action will check if a previous http response has the specified status code. If the status code does not match then this action will fail.

`Regex`:

```shell
/http status (\d+)/i
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
http status <status>
```

- **`status`**: this parameter is required is the status code to expect.

This action returns null

Example of usage:

- Check if last HTTP response has status code 200

    > http status 200

## Http ok

This action will check if a previous http response has status code in the range of 200-399. If it does not then this action will fail

`Regex`:

```shell
/http ok
```

`match signature`:

The matching is case insensitive and can appear anywhere within the sentence.

```shell
http ok
```

This action returns null

Example of usage:

- Check if last HTTP response has status code in the range 200-399

    > http ok