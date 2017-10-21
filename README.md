# importify — manage Haskell imports quickly

[![Build Status](https://travis-ci.org/serokell/importify.svg)](https://travis-ci.org/serokell/importify)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![HLint Status](https://codeclimate.com/github/serokell/importify/badges/issue_count.svg)](https://codeclimate.com/github/serokell/importify)

## _Importify_ in a nutshell.

`importify` tool helps you to manage the import section of your Haskell project modules.
GHC compiler can warn you about unused imports, and it's a good practice to remove such
imports immediately. But this becomes tedious, especially if you use explicit import lists.

_Importify_ can remove unused imports automatically.

Before importify |  After importify
:---------------:|:-----------------:
![Module with imports mess](https://user-images.githubusercontent.com/4276606/29321624-b6c2e11a-81e3-11e7-9003-da2a399c9161.png) | ![After removing all unused imports](https://user-images.githubusercontent.com/4276606/29321628-b98afb30-81e3-11e7-855f-3430fe9d250f.png)

You can use [`stylish-haskell`](https://github.com/jaspervdj/stylish-haskell) after `importify` to prettify imports.

In the future, we plan for _Importify_ to be able to:

 + Add missing imports automatically, so you won't have to manage
   imports manually at all.
 + Implement a cache server with the following features:
   + Download caches for Hackage packages to speed up _Importify_ runs.
   + Upload your caches for yet-to-be-published FOSS projects to
     make it easier to collaborate.
   + Query mappings from any module of every package to symbols
     exported by it to write your refactoring tools.
 + Convert imports between _implicit_ and _explicit_, and between
   _qualified_ and _unqualified_ forms.
 + Resolve merge conflicts in import section automatically. See an
   example of [such conflict](http://i.imgur.com/97YVCFk.png).

## Installation

Installation process assumes that you have already installed and configured the `stack`
build tool. Currently `importify` works only with projects built with `stack`.

Perform the following steps before driving:

```bash
$ git clone https://github.com/serokell/importify.git  # 1. Clone repository locally
$ cd importify                                         # 2. Step into folder
$ stack install importify\:exe\:importify              # 3. Copy executable under ~/.local/bin
```

## Usage

In short:

```bash
$ cd my-project-which-builds-with-stack
$ importify cache
$ importify file path/to/File/With/Unused/Imports.hs
```

`importify` has several commands. Most important is

```
importify --help
```

Before removing redundant imports run `importify cache`
command. Importify stores local cache for the project under the
`.importify` folder inside your project. This cache stores exported
entities for each module for every dependency and for all your local
packages. Make sure to re-run `importify cache` if you change the list
of exported functions and types in your project modules. Cache is
built incrementally; it builds dependencies only once. But if you add
dependencies or use other versions of them 
(for instance, bumping stack lts) you need to run `importify cache` again. You can
always perform `rm -rf .importify` before caching if you face any
troubles.

After the cache is built, you can use `importify file PATH_TO_FILE`
command from your project root directory. This command runs
_Importify_ on the file and prints the result in the terminal. If you
want to change a file in-place use the following command:

```
importify file -i PATH_TO_FILE
```
