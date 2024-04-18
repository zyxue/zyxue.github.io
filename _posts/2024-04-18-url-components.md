---
layout: post
title: URL components and their names
author: Zhuyi Xue
tags: network, url
---

This post explains the names of different parts of a URL. For a given URL like

```
https://blog.mysite.com:8080/marketing/parts-url;foo=bar?key1=hello&key2=world#qux
```

**Names for individual components**:

* `https`: Scheme
* `blog`: Subdomain
* `mysite`: 2nd-level domain
* `com`: Top-level domain
* `blog.mysite.com`: Host
* `8080`: Port
* `/marketing/parts-url`: Path
* `foo=bar`: Params.
* `key1=hello&key2=world`: Query
* `qux`: Fragment.

**Names for combination of multiple components**:

* `blog.mysite.com:8080`: i.e. `<host>:<port>`, Network location/Socket
* `https://blog.mysite.com:8080`: i.e. `<scheme>://<host>:<port>`,
  [Origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)

**Parsing example in Python**:

```
>>> from urllib.parse import urlparse
>>> urlparse("https://blog.example.com:8080/marketing/parts-url;foo=bar?key1=hello&key2=world#qux")
ParseResult(scheme='https', netloc='blog.example.com:8080', path='/marketing/parts-url', params='foo=bar', query='key1=hello&key2=world', fragment='qux')
```

**References:**
* [RFC 1808 Relative Uniform Resource
  Locators](https://datatracker.ietf.org/doc/html/rfc1808.html)
  * Generic-RL syntax: `<scheme>://<net_loc>/<path>;<params>?<query>#<fragment>`