scratchpad.js
=============

A utility to help reuse expensive objects inside performance critical code


Getting Started
===============

Install as a script tag.

```html
<script src="path/to/scratchpad.min.js"></script>
```

OR

use as AMD module

```javascript
require([ 'scratchpad '], function( Scratchpad ){
    // code...
});
```

OR

use with nodejs.

```javascript
var Scratchpad = require('scratchpad');
```


Usage
=====

Register your recyclable object.

```javascript
// register a hypothetical vector class...
Scratchpad.register('vector', Vector);
```

Use a scratchpad in a performance critical function.

```javascript
var myAlg = function( scratch, arg1, arg2, ... ){
    var scratch = Scratchpad()
        ,vec = scratch.vector().set( 0, 0 ) // need to reinitialize... it's recycled!
        ;

    // ...

    return scratch.done( result );
};

// later...
while( awesome ){
    myAlg( arg1, arg2, ... );
}
```

Or... better yet...

Wrap a performance critical function with Scratchpad to get a scratch instance and clean up automatically (don't need to call done.)

```javascript
var myAlg = Scratchpad(function( scratch, arg1, arg2, ... ){
    var vec = scratch.vector().set( 0, 0 ); // need to reinitialize... it's recycled!

    //...

    return result;
});

// later...
while( awesome ){
    myAlg( arg1, arg2, ... );
}
```

Performance Test
================

http://jsperf.com/scratchpad-js
