---
title: JSON API Cheatsheet
layout: article
categories: backend
excerpt: A quick reference to help design JSON-API apis
tags: [ "json-api", "backend" ]
---


# JSON-API Cheatsheet

## Introduction

Properly organizing API endpoints and JSON-serialized data responses it's pretty difficult, especially when you work on a project that will extended and improved by a larger community.

I'm getting used to stick to the [JSON-API](https://jsonapi.org/) - version 1.0, considered stable at the time of writing - when defining APIs. Few conventions make your API clearer and many libraries also support serializing and deserializing JSON APIs with ease.

> JSON:API is a specification for how a client should request that resources be fetched or modified, and how a server should respond to those requests.

> JSON:API is designed to minimize both the number of requests and the amount of data transmitted between clients and servers. This efficiency is achieved without compromising readability, flexibility, or discoverability.

Adopting the full standard is maybe too much for this project.

In short the API must respect few conventions:

- Content-type must always be `Content-Type: application/vnd.api+json`
- JSON Payloads have a standard structure, see section below
- Responses use JSON but add typing checks i.e. JSON Schema types
- Relationships are modeled in the response
- Standard error codes

## Request and response headers

#### Request

```http
GET /articles HTTP/1.1
Accept: application/vnd.api+json
```

#### Response

```http
HTTP/1.1 201 Created
Content-Type: application/vnd.api+json
```

## Response codes

| Code | Verbose   | Description                                 |
| ---- | --------- | ------------------------------------------- |
| 200  | OK        | resource / relationship fetched correctly   |
| 404  | Not found | resource / relationship not found           |
| 201  | Created   | POST request created a resource             |
| 202  | Accepted  | POST request accepted but not yet processed |
| 403  | Forbidden | unsupported request                         |
| 409  | Conflict  | duplicate operation with the same client id |

## Resource identifier

Any resource must contain:

- id
- type

and optionally

- attributes
- relationships

```json
{
  "type": "articles",
  "id": "1",
  "attributes": {
    "title": "Rails is Omakase"
  },
  "relationships": {
    "author": {
      "links": {
        "self": "/articles/1/relationships/author",
        "related": "/articles/1/author"
      },
      "data": { "type": "people", "id": "9" }
    }
  }
}
```

## Resource structure

Responses contains always a toplevel JSON Object

And at least one of the following:

- data: the document’s “primary data”
- errors: an array of error objects
- meta: a meta object that contains non-standard meta-information.

it may contain also:

- jsonapi: an object describing the server’s implementation
- links: a links object related to the primary data.
- included: an array of resource objects that are related to the primary data and/or each other (“included resources”).

## Relationships

Relationship resource objects must at least contain one of:

- links: a links object containing at least one of the following:
  - self: a link for the relationship itself (a “relationship link”). This link allows the client to directly manipulate the relationship. For example, removing an author through an article’s relationship URL would disconnect the person from the article without deleting the people resource itself. When fetched successfully, this link returns the linkage for the related resources as its primary data. (See Fetching Relationships.)
  - related: a related resource link
- data: resource linkage
- meta: a meta object that contains non-standard meta-information about the relationship.

```
{
  "type": "articles",
  "id": "1",
  "attributes": {
    "title": "Rails is Omakase"
  },
  "relationships": {
    "author": {
      "links": {
        "self": "http://example.com/articles/1/relationships/author",
        "related": "http://example.com/articles/1/author"
      },
      "data": { "type": "people", "id": "9" }
    }
  },
  "links": {
    "self": "http://example.com/articles/1"
  }
}
```

## Compound Documents

The "included" field in the resource description is used to add related resources in order to minimize the number of requests.

**NOTE**

Included resources must contain a `links.self` that is included somewhere else in the response resources.

```javascript
// excerpt
{ "included": [{
    "type": "people",
    "id": "9",
    "attributes": {
      "first-name": "Dan",
      "last-name": "Gebhardt",
      "twitter": "dgeb"
    },
    "links": {
      "self": "http://example.com/people/9"
    }
  }, {
    "type": "comments",
    "id": "5",
    "attributes": {
      "body": "First!"
    },
    "relationships": {
      "author": {
        "data": { "type": "people", "id": "2" }
      }
    },
    "links": {
      "self": "http://example.com/comments/5"
    }
  }]
}
/// end excerpt
```

## Accessing collections

```http
GET /articles
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
```

```json
{
  "links": {
    "self": "http://example.com/articles"
  },
  "data": [
    {
      "type": "articles",
      "id": "1",
      "attributes": {
        "title": "JSON:API paints my bikeshed!"
      }
    },
    {
      "type": "articles",
      "id": "2",
      "attributes": {
        "title": "Rails is Omakase"
      }
    }
  ]
}
```

## Pagination

Links are used for pagination, containing resources expressed as urls

- first: the first page of data
- last: the last page of data
- prev: the previous page of data
- next: the next page of data

The value `null` must be used if the specific key is not available

The `page` **query parameter** should be used for pagination. Also ok `page[per_page]` etc.

## Filtering

The `filter` **query parameter** should be used. Nothing else is specified.

## Accessing individual resources

```http
GET /articles/1
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
```

```json
{
  "links": {
    "self": "http://example.com/articles/1"
  },
  "data": {
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "JSON:API paints my bikeshed!"
    },
    "relationships": {
      "author": {
        "links": {
          "related": "http://example.com/articles/1/author"
        }
      }
    }
  }
}
```

## Accessing relationships

```http
GET /articles/1/relationships/comments HTTP/1.1
Accept: application/vnd.api+json
```

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
```

```json
{
  "links": {
    "self": "/articles/1/relationships/author",
    "related": "/articles/1/author"
  },
  "data": {
    "type": "people",
    "id": "12"
  }
}
```

## Creating resources

```http
POST /photos HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": {
    "type": "photos",
    "attributes": {
      "title": "Ember Hamster",
      "src": "http://example.com/images/productivity.png"
    },
    "relationships": {
      "photographer": {
        "data": { "type": "people", "id": "9" }
      }
    }
  }
}
```

## Updating resources

```http
PATCH /articles/1 HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": {
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "To TDD or Not"
    }
  }
}
```

## Updating relationships

```http
PATCH /articles/1 HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": {
    "type": "articles",
    "id": "1",
    "relationships": {
      "author": {
        "data": { "type": "people", "id": "1" }
      }
    }
  }
}
```

## Updating relationships - alternative way

One-to-one

```http
PATCH /articles/1/relationships/author HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": { "type": "people", "id": "12" }
}
```

One-to-many and Many-to-many

```http
PATCH /articles/1/relationships/tags HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": [{ "type": "tags", "id": "2" }, { "type": "tags", "id": "3" }]
}
```

## Deleting relationships

```http
DELETE /articles/1/relationships/comments HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json
```

```json
{
  "data": [
    { "type": "comments", "id": "12" },
    { "type": "comments", "id": "13" }
  ]
}
```

## Errors

Errors must be returned as an Array `errors` key on the top-level object.

Each error can include:

- id: optional unique identifier
- status: HTTP status code
- code
- title
- detail
- links
  - about: link to the page describing this specific error

# References

- [JSON API Specification](https://jsonapi.org)
