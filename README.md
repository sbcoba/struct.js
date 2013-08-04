struct.js
=========

C-Like Struct for JavaScript.
* Support multiple typed array
* Convert javascript data structure to arrayBuffer
* Convert arrayBuffer to javascript data structure
* Recycling of javascript structure
* Cool updating values of typed array
* Teeny tiny library
* Super easy to use

### Usage
 ```javascript
/**
 * create struct
 *
 * arg1 = data object
 * arg2 = default value(optional, default 0)
 * arg3 = endian(optional, default true)
 */

var struct = new Struct({
  foo: 'uint8',
  bar: ['int8', 127],
  baz: {
    qux: ['uint16', 65535]
  }
}, 0, true);

  
/**
 * write arraybuffer from javascript object
 *
 * arg1 = object(optional)
 */

var ab = struct.write();
// => ArrayBuffer {byteLength: 4, slice: function}


/**
 * read data object from arraybuffer
 *
 * arg1 = arraybuffer
 * arg2 = offset(optinal, default 0)
 */

struct.read(ab);
// => Obejct {foo: 0, bar: 127, baz: {qux: 65535}}


/**
 * update data with write and read
 */

var ab2 = struct.write({foo: 255, baz: {qux: 0}};
// => ArrayBuffer {byteLength: 4, slice: function}

struct.read(ab2);
// => Obejct {foo: 255, bar: 127, baz: {qux: 0}}


/**
 * read data with custom offset
 */

var struct = new Struct({
  sig: 'uint8',
  mimeType: 'uint8',
  id: 'uint16',
  byteLength: 'uint32'
});

...

function parseBinary(count, chunk, callback) {
  var offset = 0;
  for (var index = 0; index < count; index++) {
    var meta = struct.read(chunk, offset)
      , buffer = chunk.slice(
        offset += struct.byteLength,
        offset += meta.byteLength
      );

    callback(meta, new Uint8Array(buffer));
  }
}
```
Have fun!

### TODO
* Support data type keywords of C
* Support mult-byte typed arrays
* Support unicode encoding/decoding
* Support merge other data structure
* Support append arrayBuffer to body frmae

### License

MIT <3
