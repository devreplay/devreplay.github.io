---
title: DevReplay
layout: default
subtitle: A linter based on developer own coding convention.
---

# Quick start

You can install DevReplay using [npm](https://nodejs.org) or yarn:

```sh
npm install -g devreplay
# or
yarn global add devreplay
```

You should then make a `.devreplay.json` file.
Here is the example.

```json
[
  {
    "before": [
      "(?<tmp>.+)\\s*=\\s*(?<a>.+)",
      "\\k<a>\\s*=\\s*(?<b>.+)",
      "\\k<b>\\s*=\\s*\\k<tmp>"
    ],
    "after": ["$2, $3 = $3, $2"],
    "isRegex": true
  }
]
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

Check out [the full usage guide](https://github.com/devreplay/devreplay/blob/master/README.md) to learn more.
