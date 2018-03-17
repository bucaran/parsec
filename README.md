# Getopts

[![Travis CI](https://img.shields.io/travis/jorgebucaran/getopts/master.svg)](https://travis-ci.org/jorgebucaran/getopts)
[![Codecov](https://img.shields.io/codecov/c/github/jorgebucaran/getopts/master.svg)](https://codecov.io/gh/jorgebucaran/getopts)
[![npm](https://img.shields.io/npm/v/getopts.svg)](https://www.npmjs.org/package/getopts)

Getopts is a Node.js CLI options parser. It's designed according to the [Utility Conventions](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) so that your programs behave like typical UNIX utilities effortlessly — without sacrificing developer experience.

Need for speed? Getopts is optimized for runtime performance and runs 10 to 20 times faster than alternatives according to our [benchmarks](/bench).

## Installation

<pre>
npm i <a href="https://www.npmjs.com/package/getopts">getopts</a>
</pre>

## Usage

Use getopts to parse the arguments passed into your program from the command line.

<pre>
$ <a href="./example/demo">example/demo</a> --super=sonic -xU9000 -- game over
</pre>

```js
const getopts = require("getopts")

const options = getopts(process.argv.slice(2), {
  alias: {
    s: "super",
    U: "ultra"
  },
  default: {
    turbo: true
  }
})
```

Getopts expects an array of arguments and options object (optional) and returns an object where you can look up the argument keys and their values.

```json
{
  _: ["game", "over"],
  x: true,
  s: "sonic",
  U: "9000",
  super: "sonic",
  ultra: "9000",
  turbo: true
}
```

## API

### getopts(argv, options)

#### argv

An array of arguments to parse. See [`process.argv`](https://nodejs.org/docs/latest/api/process.html#process_process_argv).

Arguments that begin with one or two dashes are called options or flags. Options may have one or more [aliases](#optsalias). The underscore key stores operands. Operands include non-options, the single dash `-` and all the arguments after `--`.

#### options.alias

An object of option aliases. An alias can be a string or an array of strings.

```js
getopts(["-U"], {
  alias: {
    U: ["u", "ultra"]
  }
}) //=> { _:[], u:true, U:true, ultra:true }
```

#### options.boolean

An array of options that should be parsed as booleans. In the example, by indicating <samp>U</samp> to be a boolean option, <samp>1</samp> is parsed as an operand.

```js
getopts(["-U", 1], {
  boolean: ["U"]
}) //=> { _:[1], U:true }
```

#### options.default

An object of default values for missing options.

```js
getopts(["-U"], {
  default: {
    turbo: true
  }
}) //=> { _:[], U:true, turbo:true }
```

#### options.unknown

A function that runs for every unknown option. Return `false` to dismiss the option.

```js
getopts(["-abc"], {
  unknown: option => "a" === option
}) // => { _:[], a:true }
```

## License

Getopts is MIT licensed. See [LICENSE](LICENSE.md).
