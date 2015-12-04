> CLI parser

[![npm package][npm-ver-link]][parsec]
[![][dl-badge]][npm-pkg-link]
[![][travis-logo]][travis]
![][mit-badge]

<a name="parsec"></a>

<br>
<p align="center">
<a href="https://github.com/bucaran/parsec/blob/master/README.md">
<img width="350px" src="https://cloud.githubusercontent.com/assets/8317250/11601058/09c3220c-9b14-11e5-82ac-1fc6d5e8538a.png">
</a>
</p>
<br>

<p align="center">
<b><a href="#install">Install</a></b>
|
<b><a href="#synopsis">Synopsis</a></b>
|
<b><a href="#usage">Usage</a></b>
|
<b><a href="#example">Example</a></b>
|
<b><a href="#hacking">Hacking</a></b>
|
<b><a href="#about">About</a></b>
</p>


## Install

```sh
npm install parsec
```

## Synopsis

_Parsec_ is a tiny [CLI parser](https://en.wikipedia.org/wiki/Command-line_interface#Arguments).

```js
// node ./index.js -abc --secret=42
parse()
```
```json
{
  "a": true,
  "b": true,
  "c": true,
  "secret": 42
}
```

Customize:

```js
// node ./index.js -s42
parse("secret", ["verbose", "V", { default: true }])
```
```json
{
  "s": 42,
  "secret": 42,
  "V": true,
  "verbose": true
}
```

## Features

+ Simple
+ Based in the [UNIX Utility Conventions](http://pubs.opengroup.org/onlinepubs/7908799/xbd/utilconv.html)
+ Custom aliases
* Default shorthands
+ Default values / types
+ Handle `--no-*` options
+ Handle unknown options

## Usage

> Parse [`process.argv`](https://nodejs.org/docs/latest/api/process.html#process_process_argv) by default.

```js
import parse from "parsec"
parse(alias1, alias2, ...)
```


+ Custom aliases

  ```js
  // node ./index.js --bar
  parse(["foo", "bar", "baz"])
  ```

  ```json
  {
    "foo": true,
    "bar": true,
    "baz": true
  }
  ```

+ Example aliases

  ```js
  "foo" // → ["f", "foo"]
  ["F", "f", "foo"]
  ["foo", { default: "./" }]
  ["baz", { default: true }]
  ```

+ Default shorthands

  ```js
  // node ./index.js -fb
  parse("foo", "bar")
  ```
  ```json
  {
    "f": true,
    "foo": true,
    "b": true,
    "bar": true,
  }
  ```

+ Default values and types

  ```js
  // node ./index.js --file
  parse(["f", "file", { default: "." }])
  ```
  ```json
  {
    "f": ".",
    "file": "."
  }
  ```

+ Handle `--no-flags`

  ```js
  // node ./index.js --no-foo --no-bar=baz
  parse()
  ```
  ```json
  {
    "foo": false,
    "no-bar": "baz"
  }
  ```

+ Handle unknown options

  ```js
  // node ./index.js --bar
  parse("foo", (option) => {
    throw new RangeError(`unknown option ${option}`) // bar
  })
  ```

+ Bare operands and arguments after `--` are added to `._`. To override:

  ```js
  parse(["_", "alias"])
  ```

+ Bind `parse` to a different source of arguments:

  ```js
  parse.call(["--foo", "--bar"], [alias1, alias2, ...])
  ```

## Example

```js
// ./node index.js -f bar -bp
parse()
```
```json
{
  "f": "bar",
  "b": true,
  "p": "./"
}
```

with custom aliases:

```js
parse("foo", "bar", ["path", { default: "./" }])
```

```json
{
  "f": "bar",
  "foo": "bar",
  "b": true,
  "bar": true,
  "p": "./",
  "path": "./"
}
 ```

# Hacking

```sh
git clone https://github.com/bucaran/parsec
cd parsec
npm run setup
```


[license]: http://opensource.org/licenses/MIT
[author]: http://about.bucaran.me
[parsec]: https://www.github.com/bucaran/parsec
[mit-badge]: https://img.shields.io/badge/license-MIT-444444.svg?style=flat-square
[npm-pkg-link]: https://www.npmjs.org/package/parsec
[npm-ver-link]: https://img.shields.io/npm/v/parsec.svg?style=flat-square
[dl-badge]: http://img.shields.io/npm/dm/parsec.svg?style=flat-square
[travis-logo]: http://img.shields.io/travis/bucaran/parsec.svg?style=flat-square
[travis]: https://travis-ci.org/bucaran/parsec
[contributors]: https://github.com/bucaran/parsec/graphs/contributors
