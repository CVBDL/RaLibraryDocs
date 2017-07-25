# RALibrary REST API

## Table of Contents

* [Overview](#overview)
  * [Current Version](#current-version)
  * [Schema](#schema)
  * [Parameters](#parameters)
  * [Root Endpoint](#root-endpoint)
  * [Client Errors](#client-errors)
  * [HTTP Verbs](#http-verbs)
  * [Authentication](#authentication)
  * [Cross Origin Resource Sharing](#cross-origin-resource-sharing)
* [Users](#users)
  * [Get the authenticated user informatioin](#get-the-authenticated-user-informatioin)
  * [List the authenticated user borrowed books](#list-the-authenticated-user-borrowed-books)
  * [Borrow a book](#borrow-a-book)
  * [Return a book](#return-a-book)
* [Books](#books)
  * [List books](#list-books)
  * [Get a single book](#get-a-single-book)
  * [Create a book](#create-a-book)
  * [Edit a book](#edit-a-book)
  * [Delete a book](#delete-a-book)

## Overview

### Current Version

The current version is `v1`.

### Schema

All API access is over HTTP. All data is sent and received as JSON.

Blank fields are included as `null` instead of being omitted.

All timestamps are returned in ISO 8601 format:

```text
YYYY-MM-DDTHH:MM:SSZ
```

### Parameters

Many API methods take optional parameters. For GET requests,any parameters not
specified as a segment in the path can be padded as an HTTP query string parameter:

```sh
curl -i http://hostname:port/ralibrary/api/books?q=JavaScript
```

### Root Endpoint

You can issue a `GET` request to the root endpoint to get all the endpoint
categories that the API supports:

```sh
curl http://hostname:port/ralibrary/api
```

### Client Errors

TBD

### HTTP Verbs

EagleEye Platform API will try to use appropriate HTTP verbs for each action.

Verb `PATCH` is an uncommon HTTP verb, so use `POST` instead.

| Verb       | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| GET        | Used for retrieving resources.                                         |
| POST       | Used for creating resources or update a resource.                      |
| POST       | Used for updating resources with partial JSON data. Instead of `PATCH` |
| DELETE     | Used for deleting resources.                                           |

### Authentication

RALibrary API accepts an OAuth2 bearer JWT token in the HTTP header
`Authorization`.
Requests that require authentication will return `401 Unauthorized`.
See [how to get an access token](https://github.com/CVBDL/RAAuthenticationDocs/blob/master/rest-api.md)

```
curl -H "Authorization: Bearer JWT" http://hostname:port/ralibrary/api/books
```

### Cross Origin Resource Sharing

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests from any origin.
You can read the [CORS W3C Recommendation](http://www.w3.org/TR/cors/).

This is an example:

```sh
curl -i http://ra.com/ralibrary/api/books -H "Origin: http://example.com"
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept
Access-Control-Allow-Methods: GET, PUT, POST, DELETE, OPTIONS
```

## Users

### Get the authenticated user informatioin

```text
GET /api/user/details
```

#### Example

```json
{
  "IsAdmin": false
}
```

### List the authenticated user borrowed books

```text
GET /ralibrary/api/user/books

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODMyMDAwMDAsImVtYWlsIjoicGF0cmljay56aG9uZ0BleGFtcGxlLmNvbSJ9.E41uEnlFDhLk_05ftd95xNdbxSuVpO1X1TTJ5uJDStE
```

#### Example

```json
[
  {
    "Id": 1,
    "BarCode": "P01",
    "Title": "Professional JavaScript for Web"
  }
]
```

### Borrow a book

```text
POST /ralibrary/api/user/books

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODMyMDAwMDAsImVtYWlsIjoicGF0cmljay56aG9uZ0BleGFtcGxlLmNvbSJ9.E41uEnlFDhLk_05ftd95xNdbxSuVpO1X1TTJ5uJDStE
```

#### Input

| Name  | Type   | Description         |
| ----- | ------ | ------------------- |
| Id    | string | The id of the book. |

#### Response

```text
HTTP/1.1 204 No Content
```

### Return a book

```text
DELETE /ralibrary/api/user/books/:Id

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODMyMDAwMDAsImVtYWlsIjoicGF0cmljay56aG9uZ0BleGFtcGxlLmNvbSJ9.E41uEnlFDhLk_05ftd95xNdbxSuVpO1X1TTJ5uJDStE
```

#### Response

```text
HTTP/1.1 204 No Content
```

## Books

### List books

```
GET /ralibrary/api/books
```

#### Example

```json
[
  {
    "Id": 1,
    "BarCode": "P01",
    "Title": "Professional JavaScript for Web"
  },
  {
    "Id": 2,
    "BarCode": "P02",
    "Title": "ASP.NET4.5 entry Classic - ( 7th Edition )(Chinese Edition)"
  }
]
```

### Get a single book

```
GET /ralibrary/api/books/:Id
```

#### Example

```json
{
  "Id": 1,
  "BarCode": "P01",
  "Title": "Professional JavaScript for Web"
}
```

### Create a book

```
POST /ralibrary/api/books
```

#### Input

| Name           | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| BarCode        | string | Internal book code. Unique.  |
| Title          | string | Book title.                  |

#### Example

```json
{
  "Id": 3,
  "BarCode": "P03",
  "Title": "C++ Primer (5th Edition)"
}
```

### Edit a book

```
POST /ralibrary/api/books/:Id
```

#### Input

| Name           | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| BarCode        | string | Internal book code. Unique.  |
| Title          | string | Book title.                  |

#### Example

```json
{
  "Id": 3,
  "BarCode": "P03",
  "Title": "C++ Primer (5th Edition)"
}
```

### Delete a book

```text
DELETE /ralibrary/api/books/:Id
```

#### Response

```text
HTTP/1.1 204 No Content
```
