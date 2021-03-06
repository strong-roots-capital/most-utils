# `most-buffer` #

[![Version](https://img.shields.io/npm/v/most-buffer.svg?style=flat-square)](https://npmjs.org/package/most-buffer) [![License](https://img.shields.io/badge/license-BSD--3--Clause-42358A.svg?style=flat-square)](https://github.com/craft-ai/most-utils/blob/master/LICENSE)

Gather [most js](https://github.com/cujojs/most) streams events into buffers. Buffers are emitted according to a given event count or gather the complete stream if no count is given.

## Installation ##

Using npm:

```console
$ npm install --save most-buffer
```

In Node.js:

```js
const buffer = require('most-buffer');
```

## Usage ##

`stream.thru(buffer(count)) -> Stream`

```
stream:                 -a--b--c--------d--e--|
stream.thru(buffer(3)): -------[a,b,c]--------[d,e]|
```

- `count` is the size of the buffer, if undefined the full stream will be buffered before being emitted as an array.

## Examples ##

```js
const most = require('most');
const buffer = require('most-buffer');

// Logs
// [1, 2, 3, 4]
// [5, 6, 7, 8]
// [9]
most.iterate(x => x + 1, 0)
  .take(9) // 9 first numbers
  .thru(buffer(4)) // In buffer of 4 or less
  .observe(x => console.log(x))
```

```js
// Logs
// [1, 2, 3, 4, 5, 6, 7, 8, 9]
most.iterate(x => x + 1, 0)
  .take(9) // 9 first numbers
  .thru(buffer()) // Buffer the complete stream
  .observe(x => console.log(x))
```
