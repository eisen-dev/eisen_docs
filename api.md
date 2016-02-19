---
title: Eisen API v1.0
---

# Overview

This describes the resources that make up the Eisen API v1.0. If you have any problems or requests please contact
[support](alice.ferrazzi@gmail.com).

{:toc}

## Current Version

By default, all requests receive the **v1.0** [version](/v1.0/versions) of the API.
with basic auth

## Schema

All API access is over HTTP/HTTPS, and accessed from the `yourdomain.com`
domain.  All data is sent and received as JSON.  

``` command-line
$ curl -u username:password -i http://localhost:5000/eisen/api/v1.0/agent

>HTTP/1.0 200 OK
>Content-Type: application/json
>Content-Length: 156
>Server: Werkzeug/0.10.1 Python/2.7.10
>Date: Thu, 05 Nov 2015 06:47:20 GMT

{
    "agent": [
        {
            "module": "Ansible",
            "uri": "/eisen/api/v1.0/host/1",
            "version": "1.9.4"
        }
    ]
}

```

### Summary Representations

When you fetch a list of resources, the response includes a _subset_ of the
attributes for that resource. This is the "summary" representation of the
resource. (Some attributes are computationally expensive for the API to provide.
For performance reasons, the summary representation excludes those attributes.
To obtain those attributes, fetch the "detailed" representation.)

**Example**: When you get a list of target host, you get the summary
representation of each repository. Here, we fetch the list of target host owned
by our example Eisen agent:

    GET /eisen/api/v1.0/hosts

### Detailed Representations

When you fetch an individual resource, the response typically includes _all_
attributes for that resource. This is the "detailed" representation of the
resource. (Note that authorization sometimes influences the amount of detail
included in the representation.)

**Example**: When you get an individual target host, you get the detailed
representation of the target host. Here, we fetch the
target host 1 resource:

    GET /eisen/api/v1.0/host/1

The documentation provides an example response for each API method. The example
response illustrates all attributes that are returned by that method.

## Root Endpoint

You can issue a `GET` request to the root endpoint to get all the endpoint categories that the API supports:

``` command-line
$ curl https://localhost/eisen/
```

## Client Errors

There are three possible types of client errors on API calls that
receive request bodies:

1. Sending invalid JSON will result in a `400 Bad Request` response.

        HTTP/1.1 400 Bad Request
        Content-Length: 35

        {"message":"Problems parsing JSON"}

2. Sending the wrong type of JSON values will result in a `400 Bad
   Request` response.

        HTTP/1.1 400 Bad Request
        Content-Length: 40

        {"message":"Body should be a JSON object"}

3. Sending invalid fields will result in a `422 Unprocessable Entity`
   response.

        HTTP/1.1 422 Unprocessable Entity
        Content-Length: 149

        {
          "message": "Validation Failed",
          "errors": [
            {
              "resource": "Issue",
              "field": "title",
              "code": "missing_field"
            }
          ]
        }

All error objects have resource and field properties so that your client
can tell what the problem is.  There's also an error code to let you
know what is wrong with the field.  These are the possible validation error
codes:


Error Name | Description
-----------|-----------|
`missing` | This means a resource does not exist.
`missing_field` | This means a required field on a resource has not been set.
`invalid` | This means the formatting of a field is invalid.  The documentation for that resource should be able to give you more specific information.
`already_exists` | This means another resource has the same value as this field.  This can happen in resources that must have some unique key (such as Label names).

Resources may also send custom validation errors (where `code` is `custom`). Custom errors will always have a `message` field describing the error, as well as a `documentation_url` field pointing to some content that might help you resolve the error.

## HTTP Redirects

API v1.0 uses HTTP redirection where appropriate. Clients should assume that any
request may result in a redirection. Receiving an HTTP redirection is *not* an
error and clients should follow that redirect. Redirect responses will have a
`Location` header field which contains the URI of the resource to which the
client should repeat the requests.

Status Code | Description
-----------|-----------|
`301` | Permanent redirection. The URI you used to make the request has been superseded by the one specified in the `Location` header field. This and all future requests to this resource should be directed to the new URI.
`302`, `307` | Temporary redirection. The request should be repeated verbatim to the URI specified in the `Location` header field but clients should continue to use the original URI for future requests.

Other redirection status codes may be used in accordance with the HTTP 1.1 spec.

## HTTP Verbs

Where possible, API v1.0 strives to use appropriate HTTP verbs for each
action.

Verb | Description
-----|-----------
`HEAD` | Can be issued against any resource to get just the HTTP header info.
`GET` | Used for retrieving resources.
`POST` | Used for creating resources.
`PATCH` | Used for updating resources with partial JSON data.  For instance, an Issue resource has `title` and `body` attributes.  A PATCH request may accept one or more of the attributes to update the resource.  PATCH is a relatively new and uncommon HTTP verb, so resource endpoints also accept `POST` requests.
`PUT` | Used for replacing resources or collections. For `PUT` requests with no `body` attribute, be sure to set the `Content-Length` header to zero.
`DELETE` |Used for deleting resources.

## Authentication

There are three ways to authenticate through Eisen v1.0.  Requests that
require authentication will return `404 Not Found`, instead of
`403 Forbidden`, in some places.  This is to prevent the accidental leakage
of private repositories to unauthorized users.

### Basic Authentication

``` command-line
$ curl -u "username" https://localhost/
```

### Failed login limit

Authenticating with invalid credentials will return `401 Unauthorized`:

``` command-line
$ curl -i https://localhost -u foo:bar
> HTTP/1.1 401 Unauthorized
>Content-Type: text/html; charset=utf-8
>Content-Length: 19
>WWW-Authenticate: Basic realm="Authentication Required"
>Server: Werkzeug/0.10.1 Python/2.7.10
>Date: Thu, 05 Nov 2015 07:15:04 GMT

>Unauthorized Access#    
 
```