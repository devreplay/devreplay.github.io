---
layout: page
title: Docs
tagline: User guide
permalink: /docs.html
ref: docs
order: 1
---

# USER GUIDE

* [Installation and Usage](#installation-and-usage)
* [Configuration](#configuration)

## Installation and Usage

* [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=Ikuyadeu.devreplay)
* [GitHub Application](https://github.com/marketplace/dev-replay)
* [Npm package](https://www.npmjs.com/package/devreplay)
* [Language Server](https://www.npmjs.com/package/devreplay-server)

If you will use on command line interface

```sh
npm install -g devreplay
# or
yarn global add devreplay
```

Fix the your source file.
```sh
devreplay --fix yourfile.py > yourfile.py
```

```diff
- tmp = a
- a = b
- b = a
+ a, b = b, a
```

## Configuration

You should then make a `devreplay.json` file.
Here is the example.
```json
[
  {
    "condition": [
      "$3 = $1",
      "$1 = $2",
      "$2 = $3"
    ],
    "consequent": [
      "$1, $2 = $2, $1"
    ],
    "author": "Yuki Ueda",
    "description": "Value exchanging can be one line",
    "severity": "Information"
  }
]
```
* `condition`: Before changed [code snippet](https://macromates.com/manual/en/snippets) you can write more concrete like here
```json
"condition": [
    "tmp = a",
    "a = b",
    "b = tmp"
],
```
* `condition`: After changed [code snippet](https://macromates.com/manual/en/snippets) you can write more concrete like here
```json
"condition": [
    "a, b = b, a"
],
```
* `author`: Creator of rules or source for reliability
* `description`: Details of rules importaces (default: `condition` should be `consequent`)
* `severity`: How this pattern is important
    * **E**: Error
    * **W**: Warning
    * **I**: Information
    * **H**: Hint

