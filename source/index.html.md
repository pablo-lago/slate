---
title: Partners Spotcap API Reference

language_tabs:
  - shell

toc_footers:
 
includes:
  - errors

search: true
---

# Introduction

The Partners API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). Our API has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. We support [cross-origin resource sharing](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing), allowing you to interact securely with our API from a client-side web application (though you should never expose your secret API key in any public website's client-side code). [JSON](http://www.json.org) is returned by all API responses, including errors, although our [API libraries](https://stripe.com/docs/libraries).

# Foundations


## Separate Concerns

Requests and responses will be made to address a particular resource or collection. 

* Use the path to indicate identity 
* Headers to communicate metadata
* The body to transfer the contents
* Query params may be used as a means to pass header information also in edge cases

## Require Versioning
To prevent surprise, breaking changes to users, it is best to require a version be specified with all requests. Default versions should be avoided as they are very difficult, at best, to change in the future.

## Support ETags for Caching
Include an ETag header in all responses, identifying the specific version of the returned resource. This allows users to cache resources and use requests with this value in the If-None-Match header to determine if the cache should be updated.

## Divide Large Responses Across Requests with Ranges
Large responses should be broken across multiple requests using Range headers to specify when more data is available and how to retrieve it. See the Heroku Platform API discussion of Ranges for the details of request and response headers, status codes, limits, ordering, and iteration.

## Accept serialized JSON in request bodies
Accept serialized JSON on PUT/PATCH/POST request bodies, in addition to form-encoded data.

## Return appropriate status codes
Return appropriate HTTP status codes with each response. Successful responses should be coded according to this guide.

## Resource names
Use the plural version of a resource name unless the resource in question is a singleton within the system (for example, the overall status of the system might be /status). This keeps it consistent in the way you refer to particular resources.

## Downcase paths and attributes
Use downcased and dash-separated path names, for alignment with hostnames, e.g:
service-api.com/users
service-api.com/app-setups
Downcase attributes as well, but use underscore separators so that attribute names can be typed without quotes in JavaScript, e.g.:
service_class: "first"

## Support non-id dereferencing for convenience
In some cases it may be inconvenient for end-users to provide IDs to identify a resource. For example, a user may think in terms of a Heroku app name, but that app may be identified by a UUID. In these cases you may want to accept both an id or name, e.g.:
https://service.com/apps/{app_id_or_name}
https://service.com/apps/97addcf0-c182
https://service.com/apps/www-prod
Do not accept only names to the exclusion of IDs.


# Authentication

> To authorize, use this code:

```shell
# Pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`


```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

## Get a Specific Kitten

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.


### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

