---
title: "Plugins"
date: 2018-05-03T10:31:16-07:00
weight: -100
menu: "plugins-1.0"
version: "1.0"
product: "Plugins"
tags: ["sensu", "plugins", "1.0", "index"]
layout: "product-platforms"
---

## Overview

The goal of the Sensu Plugins project is to provide a set of community-driven, high-quality plugins, handlers and other code to maximize the effective use of Sensu in various types of auto-scaling and traditional environments.
You can write Sensu Plugins in any language as along as they follow the specification.
For Sensu 1.x (and Sensu Enterprise 1.x-3.x, Ruby is the prefered language based on the skeleton.
Now for go...

Much of the code is written in Ruby and uses the [`sensu-plugin` framework][0]; some also depend on additional gems or packages(e.g. `mysql2` or `libopenssl-devel`). Some are shell scripts! All languages are welcome but the preference is for Ruby for 

**Note**: Plugins are maintained by volunteers and do **not** have an SLA or similar.

## What is a Sensu plugin?

A Sensu plugin is a bundle of Sensu artifacts typically service specific.

A plugin is an executable that lets you build automated monitoring pipelines with Sensu. You can use the Bonsai Asset Index to discover and use Sensu Plugins.

Sensu Plugins for Sensu 1.x and Sensu Go 5.x use the same specification.

These artifacts typically include:

- Check scripts
- Metric scripts
- Sensu handlers
- Sensu mutators

Depending on the type of artifact you wish you wish to use they have different setup/configuration. The most common are check/metric scripts. Each plugin has self contained documentation that you should refer to for more in depth information.

## Using plugins with Sensu 1.x

To install a ruby plugin see [here](installation) for more details and refer to the plugins self contained documentation for any external dependencies such as os libraries, compilers, etc.

To setup:

- [Checks](/sensu-core/latest/guides/intro-to-checks)
- [Handlers](/sensu-core/latest/guides/intro-to-handlers)
- [Mutators](/sensu-core/latest/guides/intro-to-mutators)

### How do I contribute to plugins?

<!--TODO - overview of volunteer maintainers, opening issues/prs, volunteering to be a maintainer.-->
Visit the [contributing guide](https://github.com/sensu-plugins/community/blob/master/CONTRIBUTING.md).

## Using plugins with Sensu Go 5.x



[0]: https://github.com/sensu-plugins/sensu-plugin
