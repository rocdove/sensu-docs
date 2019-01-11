---
title: "Assets"
linkTitle: "Assets"
description: "The assets reference guide."
weight: 10
version: "5.1"
product: "Sensu Go"
platformContent: false 
menu:
  sensu-go-5.1:
    parent: reference
---

- [Asset specification](#asset-specification)
- [Using assets in Sensu](#using-assets-in-sensu)
- [Creating assets](#creating-assets)
- [Asset definition specification](#asset-definition-specification)
- [Examples](#examples)

Assets help you manage and reuse dependencies for Sensu checks, filters, mutators, and handlers.
An asset is an HTTP-accessible archive containing one or more executables.
At runtime, the Sensu backend (in the case of filter, mutator, and handler assets) or the Sensu agent (in the case of check assets) installs required assets using the specified URL.

## Asset format specification

Sensu expects assets to be a tar archive (optionally gzipped) containing one or more executables within a bin folder.
Asset executables must follow the [Sensu Plugin specification][].
Dependencies for the executables can be stored in a lib folder, and any ... can be stored within an include folder.

Sensu injects the following asset components into the execution context:

- `{PATH_TO_ASSET}/bin` is injected into the `PATH` environment variable.
- `{PATH_TO_ASSET}/lib` is injected into the `LD_LIBRARY_PATH` environment
  variable.
- `{PATH_TO_ASSET}/include` is injected into the `CPATH` environment variable.

### Example structure

{{< highlight shell >}}
sensu-example-handler_1.0.0_linux_amd64
├── CHANGELOG.md
├── LICENSE
├── README.md
└── bin
    └── executables following the Sensu Plugin specification
└── lib
    └── libraries
└── include
    └── placeholder
{{< /highlight >}}

## Dependency management
Sensu asset execulables must include all their own dependencies.
This makes it easy to write...

## Asset types

Sensu assets can provide dependencies for Sensu checks, filters, mutators, and handlers.

Plugins can provide executables for performing status or metric checks, mutators for changing data to a desired format, or handlers for performing an action on a Sensu event.

| Asset types | Description
| --- | --- |
|service check assets | Assets for use with service checks should follow the Sensu Plugin specification.
|metric check assets | Assets for use with metric checks should follow the Sensu Plugin specification.
|event filter assets | Assets for use with event filters provide dependencies for advanced filters.
|event mutator assets | Assets for use with event mutators should follow the Sensu Plugin specification.
|event handler assets | Assets for use with event handlers should follow the Sensu Plugin specification.

## Default cache directory

system  | sensu-backend                               | sensu-agent
--------|---------------------------------------------|-------------
default | `/var/cache/sensu/sensu-backend`            | `/var/cache/sensu/sensu-agent`
Windows | `C:\\ProgramData\sensu\cache\sensu-backend` | `C:\\ProgramData\sensu\cache\sensu-agent`

If the requested asset is not in the local cache, it is downloaded from the asset
URL. The Sensu backend does not currently provide any storage for assets; they
are expected to be retrieved over HTTP or HTTPS.

## Using assets in Sensu

To add an asset to Sensu, visit the [Bonsai Asset Index][] and download an asset definition (or two).
Asset definitions include the asset name, url, sha512 checksum, and any applicable platform filters.
The following example shows an asset definition for a `sensu-example-handler` plugin for Linux `amd64`.
See the [asset definition specification][] for more information about asset attributes.

{{< highlight shell >}}
sensu-example-handler-1.0.0-linux-amd64.json
{
  "type": "Asset",
  "api_version": "core/v2",
  "metadata": {
    "name": "sensu-example-handler",
    "namespace": "default",
    "labels": {},
    "annotations": {}
  },
  "spec": {
    "url": "https://github.com/sensu/sensu-example-handler/releases/download/1.0.0/sensu-example-handler_1.0.0_linux_amd64.tar.gz",
    "sha512": "5facfb0706e5e36edc5d13...",
    "filters": [
      "entity.system.os == linux",
      "entity.system.arch == amd64"
    ]
  }
}
{{< /highlight >}}

Then use sensuctl to create an asset.

{{< highlight shell >}}
sensuctl create --file sensu-sensu-pagerduty-handler-1.0.1-linux-amd64.json
{{< /highlight >}}

_PRO TIP: You can create multiple resources with a single `sensuctl create` command by including multiple resource definitions in the same file. See the [sensuctl reference][] for formatting requirements._

You can see your available assets using `sensuctl asset`.

{{< highlight shell >}}
# Show available assets
sensuctl asset list

# Show details for a specific asset
sensuctl asset info sensu-pagerduty-handler
{{< /highlight >}}

_NOTE: Assets are namespaced resources in Sensu: they must belong to a single namespace and can only be managed by users with the correct permissions for that namespace. See the [RBAC reference][] for more information._

Now that your assets are ready, you can use them to build monitoring workflows using checks, filters, mutators, and handlers.

### Check assets

Service check and metric check assets are downlaoded by the agent...



### Filter assets

### Mutator assets

### Handler assets

### Versioning

## Creating assets

To create an asset from a Sensu Plugin...


## Asset definition specification

### Top-level attributes

type         | 
-------------|------
description  | Top-level attribute specifying the [`sensuctl create`][sc] resource type. Assets should always be of type `Asset`.
required     | Required for asset definitions in `wrapped-json` or `yaml` format for use with [`sensuctl create`][sc].
type         | String
example      | {{< highlight shell >}}"type": "Asset"{{< /highlight >}}

api_version  | 
-------------|------
description  | Top-level attribute specifying the Sensu API group and version. For assets in Sensu backend version 5.0, this attribute should always be `core/v2`.
required     | Required for asset definitions in `wrapped-json` or `yaml` format for use with [`sensuctl create`][sc].
type         | String
example      | {{< highlight shell >}}"api_version": "core/v2"{{< /highlight >}}

metadata     | 
-------------|------
description  | Top-level collection of metadata about the asset, including the `name` and `namespace` as well as custom `labels` and `annotations`. The `metadata` map is always at the top level of the asset definition. This means that in `wrapped-json` and `yaml` formats, the `metadata` scope occurs outside the `spec` scope.  See the [metadata attributes reference][5] for details.
required     | Required for asset definitions in `wrapped-json` or `yaml` format for use with [`sensuctl create`][sc].
type         | Map of key-value pairs
example      | {{< highlight shell >}}"metadata": {
  "name": "check_script",
  "namespace": "default",
  "labels": {
    "region": "us-west-1"
  },
  "annotations": {
    "slack-channel" : "#monitoring"
  }
}{{< /highlight >}}

spec         | 
-------------|------
description  | Top-level map that includes the asset [spec attributes][sp].
required     | Required for asset definitions in `wrapped-json` or `yaml` format for use with [`sensuctl create`][sc].
type         | Map of key-value pairs
example      | {{< highlight shell >}}"spec": {
  "url": "http://example.com/asset.tar.gz",
  "sha512": "4f926bf4328fbad2b9cac873d117f771914f4b837c9c85584c38ccf55a3ef3c2e8d154812246e5dda4a87450576b2c58ad9ab40c9e2edc31b288d066b195b21b",
  "filters": [
    "entity.system.os == 'linux'",
    "entity.system.arch == 'amd64'"
  ]
}{{< /highlight >}}

### Spec attributes

url          | 
-------------|------ 
description  | The URL location of the asset. 
required     | true
type         | String 
example      | {{< highlight shell >}}"url": "http://example.com/asset.tar.gz"{{< /highlight >}}

sha512       | 
-------------|------ 
description  | The checksum of the asset. 
required     | true
type         | String 
example      | {{< highlight shell >}}"sha512": "4f926bf4328..."{{< /highlight >}}

filters      | 
-------------|------ 
description  | A set of [Sensu query expressions][1] used by the agent to determine if the asset should be installed. If multiple expressions are included, each expression must return true in order for the agent to install the asset.
required     | false 
type         | Array 
example      | {{< highlight shell >}}"filters": ["entity.system.os=='linux'", "entity.system.arch=='amd64'"] {{< /highlight >}}

### Metadata attributes

name         |      |
-------------|------ 
description  | The unique name of the asset, validated with Go regex [`\A[\w\.\-]+\z`](https://regex101.com/r/zo9mQU/2).
required     | true
type         | String
example      | {{< highlight shell >}}"name": "check_script"{{< /highlight >}}

| namespace  |      |
-------------|------
description  | The [Sensu RBAC namespace][2] that this asset belongs to.
required     | false
type         | String
default      | `default`
example      | {{< highlight shell >}}"namespace": "production"{{< /highlight >}}

| labels     |      |
-------------|------
description  | Custom attributes to include with event data, which can be queried like regular attributes. You can use labels to organize assets into meaningful collections that can be selected using [filters][3] and [tokens][4].
required     | false
type         | Map of key-value pairs. Keys can contain only letters, numbers, and underscores, but must start with a letter. Values can be any valid UTF-8 string.
default      | `null`
example      | {{< highlight shell >}}"labels": {
  "environment": "development",
  "region": "us-west-2"
}{{< /highlight >}}

| annotations | |
-------------|------
description  | Arbitrary, non-identifying metadata to include with event data. In contrast to labels, annotations are _not_ used internally by Sensu and cannot be used to identify assets. You can use annotations to add data that helps people or external tools interacting with Sensu.
required     | false
type         | Map of key-value pairs. Keys and values can be any valid UTF-8 string.
default      | `null`
example      | {{< highlight shell >}} "annotations": {
  "managed-by": "ops",
  "slack-channel": "#monitoring",
  "playbook": "www.example.url"
}{{< /highlight >}}

## Examples

### Asset definition

{{< highlight json >}}
{
  "type": "Asset",
  "api_version": "core/v2",
  "metadata": {
    "name": "check_script",
    "namespace": "default",
    "labels": {
      "region": "us-west-1"
    },
    "annotations": {
      "slack-channel" : "#monitoring"
    }
  },
  "spec": {
    "url": "http://example.com/asset.tar.gz",
    "sha512": "4f926bf4328fbad2b9cac873d117f771914f4b837c9c85584c38ccf55a3ef3c2e8d154812246e5dda4a87450576b2c58ad9ab40c9e2edc31b288d066b195b21b",
    "filters": [
      "entity.system.os == 'linux'",
      "entity.system.arch == 'amd64'"
    ]
  }
}
{{< /highlight >}}

[1]: ../../reference/sensu-query-expressions/
[2]: ../rbac#namespaces
[3]: ../filters
[4]: ../tokens
[5]: #metadata-attributes
[sc]: ../../sensuctl/reference#creating-resources
[sp]: #spec-attributes
