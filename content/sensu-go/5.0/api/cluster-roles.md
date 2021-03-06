---
title: "Cluster roles API"
linkTitle: "Cluster Roles API"
description: "Sensu Go cluster roles API reference documentation"
version: "5.0"
product: "Sensu Go"
menu:
  sensu-go-5.0:
    parent: api
---

- [The `/clusterroles` API endpoint](#the-clusterroles-api-endpoint)
	- [`/clusterroles` (GET)](#clusterroles-get)
	- [`/clusterroles` (POST)](#clusterroles-post)
- [The `/clusterroles/:clusterrole` API endpoint](#the-clusterrolesclusterrole-api-endpoint)
	- [`/clusterroles/:clusterrole` (GET)](#clusterrolesclusterrole-get)
  - [`/clusterroles/:clusterrole` (PUT)](#clusterrolescluster-put)
  - [`/clusterroles/:clusterrole` (DELETE)](#clusterrolesclusterrole-delete)

## The `/clusterroles` API endpoint

### `/clusterroles` (GET)

The `/clusterroles` API endpoint provides HTTP GET access to [cluster role][1] data.

#### EXAMPLE {#clusterroles-get-example}

The following example demonstrates a request to the `/clusterroles` API, resulting in
a JSON Array containing [cluster role definitions][1].

{{< highlight shell >}}
curl -s http://127.0.0.1:8080/api/core/v2/clusterroles -H "Authorization: Bearer TOKEN"
[
  {
    "name": "global-event-reader",
    "rules": [
      {
        "verbs": [
          "get",
          "list"
        ],
        "resources": [
          "events"
        ],
        "resource_names": [
          ""
        ]
      }
    ]
  }
]
{{< /highlight >}}

#### API Specification {#clusterroles-get-specification}

/clusterroles (GET)  | 
---------------|------
description    | Returns the list of cluster roles.
example url    | http://hostname:8080/api/core/v2/clusterroles
response type  | Array
response codes | <ul><li>**Success**: 200 (OK)</li><li>**Error**: 500 (Internal Server Error)</li></ul>
output         | {{< highlight shell >}}
[
  {
    "name": "global-event-reader",
    "rules": [
      {
        "verbs": [
          "get",
          "list"
        ],
        "resources": [
          "events"
        ],
        "resource_names": [
          ""
        ]
      }
    ]
  }
]
{{< /highlight >}}

### `/clusterroles` (POST)

/clusterroles (POST) | 
----------------|------
description     | Create a Sensu cluster role.
example URL     | http://hostname:8080/api/core/v2/clusterroles
payload         | {{< highlight shell >}}
{
  "name": "global-event-reader",
  "rules": [
    {
      "verbs": [
        "get",
        "list"
      ],
      "resources": [
        "events"
      ],
      "resource_names": [
        ""
      ]
    }
  ]
}
{{< /highlight >}}
response codes  | <ul><li>**Success**: 200 (OK)</li><li>**Malformed**: 400 (Bad Request)</li><li>**Error**: 500 (Internal Server Error)</li></ul>

## The `/clusterroles/:clusterrole` API endpoint {#the-clusterrolesclusterrole-api-endpoint}

### `/clusterroles/:clusterrole` (GET) {#clusterrolesclusterrole-get}

The `/clusterroles/:clusterrole` API endpoint provides HTTP GET access to [cluster role data][1] for specific `:clusterrole` definitions, by cluster role `name`.

#### EXAMPLE {#clusterrolesclusterrole-get-example}

In the following example, querying the `/clusterroles/:clusterrole` API returns a JSON Map
containing the requested [`:clusterrole` definition][1] (in this example: for the `:clusterrole` named
`global-event-reader`).

{{< highlight shell >}}
curl -s http://127.0.0.1:8080/api/core/v2/clusterroles/global-event-reader -H "Authorization: Bearer TOKEN"
{
  "name": "global-event-reader",
  "rules": [
    {
      "verbs": [
        "get",
        "list"
      ],
      "resources": [
        "events"
      ],
      "resource_names": [
        ""
      ]
    }
  ]
}
{{< /highlight >}}

#### API Specification {#clusterrolesclusterrole-get-specification}

/clusterroles/:clusterrole (GET) | 
---------------------|------
description          | Returns a cluster role.
example url          | http://hostname:8080/api/core/v2/clusterroles/global-event-reader
response type        | Map
response codes       | <ul><li>**Success**: 200 (OK)</li><li> **Missing**: 404 (Not Found)</li><li>**Error**: 500 (Internal Server Error)</li></ul>
output               | {{< highlight json >}}
{
  "name": "global-event-reader",
  "rules": [
    {
      "verbs": [
        "get",
        "list"
      ],
      "resources": [
        "events"
      ],
      "resource_names": [
        ""
      ]
    }
  ]
}
{{< /highlight >}}

### `/clusterroles/:clusterrole` (PUT) {#clusterrolesclusterrole-put}

#### API Specification {#clusterrolesclusterrole-put-specification}

/clusterroles/:clusterrole (PUT) | 
----------------|------
description     | Create or update a Sensu cluster role.
example URL     | http://hostname:8080/api/core/v2/clusterroles/global-event-reader
payload         | {{< highlight shell >}}
{
  "name": "global-event-reader",
  "rules": [
    {
      "verbs": [
        "get",
        "list"
      ],
      "resources": [
        "events"
      ],
      "resource_names": [
        ""
      ]
    }
  ]
}
{{< /highlight >}}
response codes  | <ul><li>**Success**: 201 (Created)</li><li>**Malformed**: 400 (Bad Request)</li><li>**Error**: 500 (Internal Server Error)</li></ul>

### `/clusterroles/:clusterrole` (DELETE) {#clusterrolesclusterrole-delete}

#### API Specification {#clusterrolesclusterrole-delete-specification}

/clusterroles/:clusterrole (DELETE) | 
--------------------------|------
description               | Removes a cluster role from Sensu given the cluster role name.
example url               | http://hostname:8080/api/core/v2/clusterroles/global-event-reader
response codes            | <ul><li>**Success**: 202 (Accepted)</li><li>**Missing**: 404 (Not Found)</li><li>**Error**: 500 (Internal Server Error)</li></ul>

[1]: ../../reference/rbac
