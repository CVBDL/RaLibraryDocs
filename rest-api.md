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
* [Books open API proxy](#books-open-api-proxy)

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

Authorization is required to use this API.

```text
GET /api/user/details
```

#### Example

```json
{
  "Email": "patrick@example.com",
  "Name": "Patrick Zhong",
  "IsAdmin": true
}
```

### List the authenticated user borrowed books

Authorization is required to use this API.

```text
GET /ralibrary/api/user/books

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Example

```json
[
  {
    "Id": 1,
    "Code": "P001",
    "ISBN10": "0596008031",
    "ISBN13": "9780596008031",
    "Title": "Designing Interfaces",
    "Subtitle": "Patterns for Effective Interaction Design",
    "Authors": "Jenifer Tidwell",
    "Publisher": "\"O'Reilly Media, Inc.\"",
    "PublishedDate": "2005-11-21",
    "Description": "Provides information on designing easy-to-use interfaces....",
    "PageCount": 331,
    "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABIRE=",
    "IsBorrowed": true
  },
  {
    "Id": 2,
    "Code": "P002",
    "ISBN10": "7111348664",
    "ISBN13": "9787111348665",
    "Title": "以用户为中心的产品设计",
    "Subtitle": "",
    "Authors": "Jesse James Garrett",
    "Publisher": null,
    "PublishedDate": "2011",
    "Description": null,
    "PageCount": 191,
    "ThumbnailLink": null,
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABAdI=",
    "IsBorrowed": true
  },
  {
    "Id": 3,
    "Code": "P003",
    "ISBN10": "7115313083",
    "ISBN13": "9787115313089",
    "Title": "Designing Interfaces",
    "Subtitle": "Patterns for Effective Interaction Design",
    "Authors": "[美] Susan Weinschenk",
    "Publisher": "人民邮电出版社",
    "PublishedDate": "2013",
    "Description": "本书出自国际知名的设计心理学专家之手,讨论了设计师必须知道的100个心理学问题.",
    "PageCount": 236,
    "ThumbnailLink": null,
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABAdM=",
    "IsBorrowed": true
  }
]
```

### Borrow a book

Authorization is required to use this API.

```text
POST /ralibrary/api/user/books/:Id

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Response

```text
HTTP/1.1 204 No Content
```

### Return a book

Authorization is required to use this API.

```text
DELETE /ralibrary/api/user/books/:Id

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Response

```text
HTTP/1.1 204 No Content
```

## Books

### List books

No authorization required to use this API.

```
GET /ralibrary/api/books
```

#### Example

```json
[
  {
    "Id": 1,
    "Code": "P001",
    "ISBN10": "0596008031",
    "ISBN13": "9780596008031",
    "Title": "Designing Interfaces",
    "Subtitle": "Patterns for Effective Interaction Design",
    "Authors": "Jenifer Tidwell",
    "Publisher": "\"O'Reilly Media, Inc.\"",
    "PublishedDate": "2005-11-21",
    "Description": "Provides information on designing easy-to-use interfaces.",
    "PageCount": 331,
    "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABAdE=",
    "IsBorrowed": false
  },
  {
    "Id": 2,
    "Code": "P002",
    "ISBN10": "7111348664",
    "ISBN13": "9787111348665",
    "Title": "以用户为中心的产品设计",
    "Subtitle": "",
    "Authors": "Jesse James Garrett",
    "Publisher": null,
    "PublishedDate": "2011",
    "Description": null,
    "PageCount": 191,
    "ThumbnailLink": null,
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABAdI=",
    "IsBorrowed": false
  },
  {
    "Id": 5,
    "Code": "P005",
    "ISBN10": "0596554877",
    "ISBN13": "9780596554873",
    "Title": "JavaScript: The Good Parts",
    "Subtitle": "The Good Parts",
    "Authors": "Douglas Crockford",
    "Publisher": "\"O'Reilly Media, Inc.\"",
    "PublishedDate": "2008-05-08",
    "Description": "Most programming languages contain good and bad parts, but JavaScript has more than its share of the bad, having been developed and released in a hurry before it could be refined. This authoritative book scrapes away these bad features to reveal a subset of JavaScript that's more reliable, readable, and maintainable than the language as a whole—a subset you can use to create truly extensible and efficient code. Considered the JavaScript expert by many people in the development community, author Douglas Crockford identifies the abundance of good ideas that make JavaScript an outstanding object-oriented programming language-ideas such as functions, loose typing, dynamic objects, and an expressive object literal notation. Unfortunately, these good ideas are mixed in with bad and downright awful ideas, like a programming model based on global variables. When Java applets failed, JavaScript became the language of the Web by default, making its popularity almost completely independent of its qualities as a programming language. In JavaScript: The Good Parts, Crockford finally digs through the steaming pile of good intentions and blunders to give you a detailed look at all the genuinely elegant parts of JavaScript, including: Syntax Objects Functions Inheritance Arrays Regular expressions Methods Style Beautiful features The real beauty? As you move ahead with the subset of JavaScript that this book presents, you'll also sidestep the need to unlearn all the bad parts. Of course, if you want to find out more about the bad parts and how to use them badly, simply consult any other JavaScript book. With JavaScript: The Good Parts, you'll discover a beautiful, elegant, lightweight and highly expressive language that lets you create effective code, whether you're managing object libraries or just trying to get Ajax to run fast. If you develop sites or applications for the Web, this book is an absolute must.",
    "PageCount": 172,
    "ThumbnailLink": "http://books.google.com/books/content?id=PXa2bby0oQ0C&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
    "CreatedDate": "2017-09-21T13:49:45.243",
    "RowVersion": "AAAAAAABAdU=",
    "IsBorrowed": false
  }
]
```

### Get a single book

No authorization required to use this API.

```
GET /ralibrary/api/books/:Id
```

#### Example

```json
{
  "Id": 1,
  "Code": "P001",
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "Jenifer Tidwell",
  "Publisher": "\"O'Reilly Media, Inc.\"",
  "PublishedDate": "2005-11-21",
  "Description": "Provides information on designing easy-to-use interfaces.",
  "PageCount": 331,
  "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
  "CreatedDate": "2017-09-21T13:49:45.243",
  "RowVersion": "AAAAAAABAdE=",
  "IsBorrowed": false
}
```

### Create a book

Authorization is required to use this API.

```
POST /ralibrary/api/books

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Input

| Name           | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| Code           | string | Internal book code. Unique.  |
| ISBN10         | string | ISBN 10.                     |
| ISBN13         | string | ISBN 13.                     |
| Title          | string | Title.                       |
| Subtitle       | string | Subtitle.                    |
| Authors        | string | Authors.                     |
| Publisher      | string | Publisher.                   |
| PublishedDate  | string | Published date.              |
| Description    | string | Description.                 |
| PageCount      | number | Page count.                  |
| ThumbnailLink  | string | Thumbnail image link.        |

#### Example

```json
{
  "Code": "P001",
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "Jenifer Tidwell",
  "Publisher": "\"O'Reilly Media, Inc.\"",
  "PublishedDate": "2005-11-21",
  "Description": "Provides information on designing easy-to-use interfaces.",
  "PageCount": 331,
  "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api"
}
```

#### Response

```json
{
  "Id": 1,
  "Code": "P001",
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "Jenifer Tidwell",
  "Publisher": "\"O'Reilly Media, Inc.\"",
  "PublishedDate": "2005-11-21",
  "Description": "Provides information on designing easy-to-use interfaces.",
  "PageCount": 331,
  "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
  "CreatedDate": "2017-09-21T13:49:45.243",
  "RowVersion": "AAAAAAABAdE=",
  "IsBorrowed": false
}
```

### Edit a book

Authorization is required to use this API.

```
POST /ralibrary/api/books/:Id

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Input

Note: All of these fields are required, otherwith the fields will be updated to
their default values.

Note: `RowVersion` is generated by server and response to client when do a query.

| Name           | Type   | Description                  |
| -------------- | ------ | ---------------------------- |
| Id             | number | Book id.                     |
| Code           | string | Internal book code. Unique.  |
| ISBN10         | string | ISBN 10.                     |
| ISBN13         | string | ISBN 13.                     |
| Title          | string | Title.                       |
| Subtitle       | string | Subtitle.                    |
| Authors        | string | Authors.                     |
| Publisher      | string | Publisher.                   |
| PublishedDate  | string | Published date.              |
| Description    | string | Description.                 |
| PageCount      | number | Page count.                  |
| ThumbnailLink  | string | Thumbnail image link.        |
| RowVersion     | string | Used for concurrency check.  |

#### Example

```json
{
  "Id": 1,
  "Code": "P001",
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "Jenifer Tidwell",
  "Publisher": "\"O'Reilly Media, Inc.\"",
  "PublishedDate": "2005-11-21",
  "Description": "[Updated] Provides information on designing easy-to-use interfaces",
  "PageCount": 331,
  "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
  "RowVersion": "AAAAAAABAdE="
}
```

#### Response

```json
{
  "Id": 1,
  "Code": "P001",
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "Jenifer Tidwell",
  "Publisher": "\"O'Reilly Media, Inc.\"",
  "PublishedDate": "2005-11-21",
  "Description": "[Updated] Provides information on designing easy-to-use interfaces",
  "PageCount": 331,
  "ThumbnailLink": "http://books.google.com/books/content?id=1D2bAgAAQBAJ&printsec=frontcover&img=1&zoom=1&edge=curl&source=gbs_api",
  "CreatedDate": "2017-09-21T13:49:45.243",
  "RowVersion": "AAAAAAABIRE=",
  "IsBorrowed": false
}
```

### Delete a book

Authorization is required to use this API.

```text
DELETE /ralibrary/api/books/:Id

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdHJpY2tAZXhhbXBsZS5jb20ifQ.axyZPNbUPwPEuS379xURV7K6KRLFpywsOChqStxLYcA
```

#### Response

```text
HTTP/1.1 204 No Content
```

## Books open API proxy

### Get book details by ISBN

```text
GET /ralibrary/apibook/isbn/:isbn
```

#### Example

```text
GET /ralibrary/apibook/isbn/0596008031
```

OR

```text
GET /ralibrary/apibook/isbn/9780596008031
```

#### Response

```json
{
  "ISBN10": "0596008031",
  "ISBN13": "9780596008031",
  "Title": "Designing Interfaces",
  "Subtitle": "Patterns for Effective Interaction Design",
  "Authors": "",
  "Publisher": "O'Reilly Media",
  "PublishedDate": "2005-11-28",
  "Description": "Designing a good interface isn't easy. Users demand software that is well-behaved, good-looking, and easy to use. Your clients or managers demand originality and a short time to market. Your UI technology -- Web applications, desktop software, even mobile devices - may give you the tools you need, but little guidance on how to use them well. UI designers over the years have refined the art of interface design, evolving many best practices and reusable ideas. If you learn these, and understand why the best user interfaces work so well, you too can design engaging and usable interfaces with less guesswork and more confidence. \"Designing Interfaces\" captures those best practices as design patterns - solutions to common design problems, tailored to the situation at hand. Each pattern contains practical advice that you can put to use immediately, plus a variety of examples illustrated in full color. You'll get recommendations, design alternatives, and warnings on when not to use them. Each chapter's introduction describes key design concepts that are often misunderstood, such as affordances, visual hierarchy, navigational distance, and the use of color.  These give you a deeper understanding of why the patterns work, and how to apply them with more insight. A book can't design an interface for you - no foolproof design process is given here - but \"Designing Interfaces\" does give you concrete ideas that you can mix and recombine as you see fit. Experienced designers can use it as a sourcebook of ideas. Novice designers will find a roadmap to the world of interface and interaction design, with enough guidance to start using these patterns immediately.",
  "ThumbnailLink": "https://img3.doubanio.com/mpic/s7411251.jpg",
  "PageCount": 352
}
```
