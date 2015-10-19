Environment Variables
===
[![NPM version][npm-image]][npm-url] [![Build Status][travis-image]][travis-url] [![Coverage Status][codecov-image]][codecov-url] [![Dependencies][dependencies-image]][dependencies-url]

> Maps [environment variables](https://en.wikipedia.org/wiki/Environment_variable) to a configuration object.


## Installation

``` bash
$ npm install env-to-object
```


## Usage

``` javascript
var env = require( 'env-to-object' );
```

#### env( map[, options] )

Maps [environment variables](https://en.wikipedia.org/wiki/Environment_variable) to a configuration `object`.

``` javascript
var map = {
	'NODE_ENV': {
		'keypath': 'env',
		'type': 'string'
	},
	'PORT': {
		'keypath': 'server.port',
		'type': 'number'
	},
	'SSL': {
		'keypath': 'server.ssl',
		'type': 'boolean'
	},
	'LOGLEVEL': {
		'keypath': 'logger.level',
		'type': 'string'
	}
};

var out = env( map );
/*
	{
		'env': <string>,
		'server': {
			'port': <number>,
			'ssl': <boolean>
		},
		'logger': {
			'level': <string>
		}
	}
*/
```

An [environment variable](https://en.wikipedia.org/wiki/Environment_variable) mapping __must__ include a [`keypath`](https://github.com/kgryte/utils-deep-set), which is a dot-delimited `object` path. By default, this module parses an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value as a `string`. The following types are supported:

*	__string__
*	__number__
*	__integer__
*	__boolean__
*	__object__
*	__date__
*	__regexp__

The `function` accepts the following `options`:

*	__parsers__: an `object` containing [environment variable](https://en.wikipedia.org/wiki/Environment_variable) parsers.

	``` javascript
	var map = {
		'CUSTOM_TYPE': {
			'keypath': 'custom',
			'type': 'custom',
			... // => options
		}
	};

	var parsers = {
		'custom': custom
	};

	function custom( str, opts ) {
		var v = parseInt( str, 10 );
		if ( v !== v ) {
			return new TypeError( 'invalid value. Value must be an integer. Value: `' + str + '`.' );
		}
		return v * 6;
	}

	process.env[ 'CUSTOM_TYPE' ] = '5';

	var out = env( map, parsers );
	/*
		{
			'custom': 30
		}
	*/
	```


===
##### string

(__default__) Coerce an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value to a `string`. The `string` type supports the following options:

* 	__oneof__: set of allowed values.

	``` javascript
	var map = {
		'STR': {
			'keypath': 'str',
			'type': 'string',
			'oneof': [
				'beep',
				'boop',
				'bap'
			]
		}
	};

	process.env[ 'STR' ] = 'beep';
	var out = env( map );
	/*
		{
			'str': 'beep'
		}
	*/

	process.env[ 'STR' ] = 'bop';
	var out = env( map );
	// => throws
	```

===
##### number

Coerce an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value to a `number`. The `number` type supports the following options:

*	__oneof__: set of allowed values.
	
	``` javascript
	var map = {
		'NUM': {
			'keypath': 'num',
			'type': 'number',
			'oneof': [ 1, 2, 3 ]
		}
	};

	process.env[ 'NUM' ] = '2';
	var out = env( map );
	/*
		{
			'num': 2
		}
	*/

	process.env[ 'NUM' ] = 'bop';
	var out = env( map );
	// => throws
	```

*	__min__: minimum value.

	``` javascript
	var map = {
		'NUM': {
			'keypath': 'num',
			'type': 'number',
			'min': 2
		}
	};

	process.env[ 'NUM' ] = '2';
	var out = env( map );
	/*
		{
			'num': 2
		}
	*/

	process.env[ 'NUM' ] = '1';
	var out = env( map );
	// => throws
	```

* 	__max__: maximum value.

	``` javascript
	var map = {
		'NUM': {
			'keypath': 'num',
			'type': 'number',
			'max': 2
		}
	};

	process.env[ 'NUM' ] = '2';
	var out = env( map );
	/*
		{
			'num': 2
		}
	*/

	process.env[ 'NUM' ] = '3';
	var out = env( map );
	// => throws
	```

*	__emin__: exclusive minimum value.

	``` javascript
	var map = {
		'NUM': {
			'keypath': 'num',
			'type': 'number',
			'emin': 2
		}
	};

	process.env[ 'NUM' ] = '3';
	var out = env( map );
	/*
		{
			'num': 3
		}
	*/

	process.env[ 'NUM' ] = '2';
	var out = env( map );
	// => throws
	```

*	__emax__: exclusive maximum value.

	``` javascript
	var map = {
		'NUM': {
			'keypath': 'num',
			'type': 'number',
			'emax': 4
		}
	};

	process.env[ 'NUM' ] = '3';
	var out = env( map );
	/*
		{
			'num': 3
		}
	*/

	process.env[ 'NUM' ] = '4';
	var out = env( map );
	// => throws
	```

===
##### integer

Coerce an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value to an `integer`. The `integer` type supports the following options:

*	__oneof__: set of allowed values.
	
	``` javascript
	var map = {
		'INT': {
			'keypath': 'int',
			'type': 'integer',
			'oneof': [ 1, 2, 3 ]
		}
	};

	process.env[ 'INT' ] = '2';
	var out = env( map );
	/*
		{
			'int': 2
		}
	*/

	process.env[ 'INT' ] = '2';
	var out = env( map );
	// => throws
	```

*	__min__: minimum value.

	``` javascript
	var map = {
		'INT': {
			'keypath': 'int',
			'type': 'integer',
			'min': 2
		}
	};

	process.env[ 'INT' ] = '2';
	var out = env( map );
	/*
		{
			'int': 2
		}
	*/

	process.env[ 'INT' ] = '1';
	var out = env( map );
	// => throws
	```

* 	__max__: maximum value.

	``` javascript
	var map = {
		'INT': {
			'keypath': 'int',
			'type': 'integer',
			'max': 2
		}
	};

	process.env[ 'INT' ] = '2';
	var out = env( map );
	/*
		{
			'int': 2
		}
	*/

	process.env[ 'INT' ] = '3';
	var out = env( map );
	// => throws
	```

*	__emin__: exclusive minimum value.

	``` javascript
	var map = {
		'INT': {
			'keypath': 'int',
			'type': 'integer',
			'emin': 2
		}
	};

	process.env[ 'INT' ] = '3';
	var out = env( map );
	/*
		{
			'int': 3
		}
	*/

	process.env[ 'INT' ] = '2';
	var out = env( map );
	// => throws
	```

*	__emax__: exclusive maximum value.

	``` javascript
	var map = {
		'INT': {
			'keypath': 'int',
			'type': 'integer',
			'emax': 4
		}
	};

	process.env[ 'INT' ] = '3';
	var out = env( map );
	/*
		{
			'int': 3
		}
	*/

	process.env[ 'INT' ] = '4';
	var out = env( map );
	// => throws
	```

===
##### boolean

Coerce an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value to a `boolean`. The `boolean` type supports the following options:

*	__strict__: `boolean` indicating whether to accept only `true` and `false` as acceptable `boolean` strings. Default: `false`.

In non-strict mode, the following values are supported:
	-	`TRUE`
	-	`True`
	-	`true`
	-	`T`
	-	`t`
	-	`FALSE`
	-	`False`
	-	`false`
	-	`F`
	-	`f`

``` javascript
var map = {
	'BOOL': {
		'keypath': 'bool',
		'type': 'boolean'
	}
};

process.env[ 'BOOL' ] = 'TRUE';
var out = env( map );
/*
	{
		'bool': true
	}
*/

process.env[ 'BOOL' ] = 'beep';
var out = env( map );
// => throws
```

To restrict the set of allowed values, set the `strict` option to `true`.

``` javascript
var map = {
	'BOOL': {
		'keypath': 'bool',
		'type': 'boolean',
		'strict': true
	}
};

process.env[ 'BOOL' ] = 'false';
var out = env( map );
/*
	{
		'bool': false
	}
*/

process.env[ 'BOOL' ] = 'TRUE';
var out = env( map );
// => throws
```

===
##### object

[Parse](https://github.com/kgryte/utils-json-parse) an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) value as a JSON `object`. Note that a value must be valid [JSON](https://github.com/kgryte/utils-json-parse).

``` javascript
var map = {
	'OBJ': {
		'keypath': 'obj',
		'type': 'object'
	}
};

process.env[ 'OBJ' ] = '{"beep":"boop"}';
var out = env( map );
/*
	{
		'obj': {
			'beep': 'boop'
		}
	}
*/

process.env[ 'OBJ' ] = '{"beep:"boop"}';
var out = env( map );
// => throws
```

===
##### date

Coerce an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) to a `Date` object.

``` javascript
var map = {
	'DATE': {
		'keypath': 'date',
		'type': 'date'
	}
};

process.env[ 'DATE' ] = '2015-10-17';
var out = env( map );
/*
	{
		'date': <Date>
	}
*/

process.env[ 'DATE' ] = 'beep';
var out = env( map );
// => throws
```

===
##### regexp

[Parse](https://github.com/kgryte/utils-regex-from-string) an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) as a `RegExp`.

``` javascript
var map = {
	'REGEXP': {
		'keypath': 're',
		'type': 'regexp'
	}
};

process.env[ 'RE' ] = '/\\w+/';
var out = env( map );
/*
	{
		're': /\w+/
	}
*/

process.env[ 'RE' ] = 'beep';
var out = env( map );
// => throws
```


---
## Notes

*	If an [environment variable](https://en.wikipedia.org/wiki/Environment_variable) does __not__ exist, the corresponding configuration `keypath` will __not__ exist in the output `object`.

	``` javascript
	var map = {
		'UNSET_ENV_VAR': {
			'keypath': 'a.b.c'
		}
	};

	var out = env( map );
	// returns {}
	```


---
## Examples

``` javascript
var env = require( 'env-to-object' );

var map = {
	'DEFAULT': {
		'keypath': 'default'
	},
	'STR': {
		'keypath': 'str',
		'type': 'string'
	},
	'NUM': {
		'keypath': 'num',
		'type': 'number'
	},
	'BOOL': {
		'keypath': 'bool',
		'type': 'boolean',
		'strict': true
	},
	'ARR': {
		'keypath': 'arr',
		'type': 'object'
	},
	'NESTED': {
		'keypath': 'a.b.c.d',
		'type': 'object'
	},
	"DATE": {
		"keypath": "date",
		"type": "date"
	},
	"REGEX": {
		"keypath": "re",
		"type": "regexp"
	},
	"INT": {
		"keypath": "int",
		"type": "integer"
	},
	"MIN": {
		"keypath": "min",
		"type": "number",
		"min": 0
	},
	"MAX": {
		"keypath": "max",
		"type": "integer",
		"max": 1
	},
	"EMIN": {
		"keypath": "emin",
		"type": "integer",
		"emin": 1023
	},
	"EMAX": {
		"keypath": "emax",
		"type": "number",
		"emax": 65536
	},
	"ONEOF": {
		"keypath": "oneof",
		"type": "string",
		"oneof": [
			"beep",
			"boop",
			"bop"
		]
	},
	"ONEOF2": {
		"keypath": "oneof2",
		"type": "integer",
		"oneof": [
			8000,
			8080,
			9000,
			10001
		]
	}
};

process.env[ 'DEFAULT' ] = 'beep';
process.env[ 'STR' ] = 'boop';
process.env[ 'NUM' ] = '1234.5';
process.env[ 'BOOL' ] = 'true';
process.env[ 'ARR' ] = '[1,2,3,4]';
process.env[ 'NESTED' ] = '{"hello":"world"}';
process.env[ 'DATE' ] = '2015-10-18T07:00:01.000Z';
process.env[ 'REGEX'] = '/\\w+/';
process.env[ 'INT' ] = '1234';
process.env[ 'MIN' ] = '0';
process.env[ 'MAX' ] = '1';
process.env[ 'EMIN' ] = '1024';
process.env[ 'EMAX' ] = '65535.9';
process.env[ 'ONEOF' ] = 'boop';
process.env[ 'ONEOF2' ] = '9000';

var out = env( map );
/*
	{
		'default': 'beep',
		'str': 'boop',
		'num': 1234.5,
		'bool': true,
		'arr': [1,2,3,4],
		'a': {
			'b': {
				'c': {
					'd': {
						'hello': 'world'
					}
				}
			}
		},
		'date': <date>,
		're': /\w+/,
		'int': 1234,
		'min': 0,
		'max': 1,
		'emin': 1024,
		'emax': 65535.9,
		'oneof': 'boop',
		'oneof2': 9000
	}
*/
```

To run the example code from the top-level application directory,

``` bash
$ node ./examples/index.js
```

or, alternatively,

``` bash
$ DEFAULT=boop STR=beep NUM='5432.1' BOOL='false' ARR='[4,3,2,1]' NESTED='{"world":"hello"}' DATE='2015-10-19T06:59:59.000Z' REGEX='/\\.+/' MIN=0 MAX=1 EMIN=1024 EMAX='65535.9' ONEOF=bop ONEOF2=8080 node ./examples/index.js
```


---
## Tests

### Unit

Unit tests use the [Mocha](http://mochajs.org/) test framework with [Chai](http://chaijs.com) assertions. To run the tests, execute the following command in the top-level application directory:

``` bash
$ make test
```

All new feature development should have corresponding unit tests to validate correct functionality.


### Test Coverage

This repository uses [Istanbul](https://github.com/gotwarlost/istanbul) as its code coverage tool. To generate a test coverage report, execute the following command in the top-level application directory:

``` bash
$ make test-cov
```

Istanbul creates a `./reports/coverage` directory. To access an HTML version of the report,

``` bash
$ make view-cov
```


---
## License

[MIT license](http://opensource.org/licenses/MIT).


## Copyright

Copyright &copy; 2015. Athan Reines.


[npm-image]: http://img.shields.io/npm/v/env-to-object.svg
[npm-url]: https://npmjs.org/package/env-to-object

[travis-image]: http://img.shields.io/travis/kgryte/node-env-to-object/master.svg
[travis-url]: https://travis-ci.org/kgryte/node-env-to-object

[codecov-image]: https://img.shields.io/codecov/c/github/kgryte/node-env-to-object/master.svg
[codecov-url]: https://codecov.io/github/kgryte/node-env-to-object?branch=master

[dependencies-image]: http://img.shields.io/david/kgryte/node-env-to-object.svg
[dependencies-url]: https://david-dm.org/kgryte/node-env-to-object

[dev-dependencies-image]: http://img.shields.io/david/dev/kgryte/node-env-to-object.svg
[dev-dependencies-url]: https://david-dm.org/dev/kgryte/node-env-to-object

[github-issues-image]: http://img.shields.io/github/issues/kgryte/node-env-to-object.svg
[github-issues-url]: https://github.com/kgryte/node-env-to-object/issues
