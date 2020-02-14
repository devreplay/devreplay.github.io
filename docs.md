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
* [Auto Rule Learning](#auto-rule-learning)

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


You can extends your `devreplay.json` from built-in rules and your local rules.

```json
[
  {
      "extends": [
          "typescript",
          "React",
          "/your/local/rules.json"
      ]
  },
  {
    "condition": [
      "$3 = $1",
      "$1 = $2",
      "$2 = $3"
    ],
    "consequent": [
      "$1, $2 = $2, $1"
    ]
  }
]
```

## Auto Rule Learning

Use [DevReplay Pattern Generator](https://github.com/devreplay/devreplay-pattern-generator)


### 0. Cloning this repository

```sh
git clone --recursive  https://github.com/Ikuyadeu/review_pattern_gen.git
pip3 install antlr4-python3-runtime unidiff gitpython
```

### 1. Preparing config file

Making empty `config.json` file

```sh
touch config.json
```

and edit `config.json` file like berrow

* If you try first time, please check bottom example for the `Option` setting
* GitHub token can be generated from https://github.com/settings/tokens)

(If your target `CPP` organization name is `mruby`)
```json
{
    "github_token": "Your github token",
    "lang": "CPP",
    "learn_from": "master",
    "validate_by": "master",
    "projects": [
        {
            "owner": "mruby",
            "repo": "mruby",
            "branch": "master"
        }
    ],
    "all_change": true,
    "all_author": false,
    "change_size": 1000,
    "time_length": {
        "start": "2019-01-01 00:00:00",
        "end": "2020-01-01 00:00:00"
    }
}
```


### Detail of parameter

`?`: The parameter is option

* ?`github_token` (If you will get code review data): Your github token from https://github.com/settings/tokens
* `lang`: Your Target Language (Python, Ruby, Java, JavaScript, CPP, PHP)
* `learn_from` and `validate_by`: Target of learning and validating (pulls or master)
* `projects`: Projects that you want to learn
    * `owner`: Project owner name
    * `repo`: Project repository name
    * ?`branch` (default is `master`): Project branch name
* ?`projects_path`: If you want to devide projects contents, you can write projects information on the other file
* ?`all_change`(default is False): Will you get all commits or not
* ?`change_size` (if all_change is false, default is 100):  Number of change that you collect
* ?`time_length` (default is all changes):
    * ?`start` (If `end` is defined, default is 1 years ago):
    * ?`end` (If `start` is defined, default is today):
* ?`all_author`?(default is True): :You will get all authors change or not (True or False) 
* ?`authors` (if all_author is false): choose target authors' name and github id
    * `git`: author git name
    * `github`: author github id


### 2. Collecting training data set

```sh
chmod +x make_rules.sh
sh make_rules.sh
```

`make_rules.sh` will runnning `collect_changes.py` and `test_rules.py`

Finally change `data/changes/yourproject2.json` to `devreplay.json`