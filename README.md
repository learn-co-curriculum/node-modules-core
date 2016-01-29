# Main Core Modules

## Overview

This lesson will cover the usage of the core Node modules. Also, we'll list the main core modules which will be useful during this course.

## Objectives

1. Provide a list of main core modules and their purpose
1. Describe fs and its main methods
1. Describe url and its main methods
1. Describe path and path.join

## Core Modules

As discussed previously, Node is a platform or an environment in which we build applications. You saw that modularized code is better and how you can write your own modules. With modules, developers from different team and different companies can share their code (what is called open-source community). This is great but how do you get the code? Imagine a simple thing like parsing a path, i.e., we have a string like `/home/user/azat/program.js` and we need to extract the extension, base, file name, etc.

You would have to write a code which uses Regular Expressions like this:

```js
// Split a filename into [root, dir, basename, ext], unix version
// 'root' is just a slash, or nothing.
const splitPathRe =
    /^(\/?|)([\s\S]*?)((?:\.{1,2}|[^\/]+?|)(\.[^.\/]*|))(?:[\/]*)$/;
var posix = {}

function posixSplitPath(filename) {
  const out = splitPathRe.exec(filename);
  out.shift()
  return out
}

posix.parse = function(pathString) {
  var allParts = posixSplitPath(pathString);
  return {
    root: allParts[0],
    dir: allParts[0] + allParts[1].slice(0, -1),
    base: allParts[2],
    ext: allParts[3],
    name: allParts[2].slice(0, allParts[2].length - allParts[3].length)
  }
}
```

This code is only for Unix-like systems (Linux, Mac OS X, etc.) and for Windows you would have to write a different parser. Do you realize that every time you want to access a file you would need something like this? Probably you won't copy/paste the code, but have a module. If you need this module so often maybe it's a good idea to make it part of the system? That's why creators of Node, packaged this path parser and other utilities and libraries that are likely to be used in most Node projects together into the platform itself. These packaged modules called core modules.

Even more so, more complex core modules like `net` depend on a simpler core modules like `path`. 
You might wonder how someone would use a core module? Import it with `require()` by passing a name. You want to store the reference in a variable:

```js
var path = require('path')
console.log(path.parse('/home/user/azat/program.js'))
```

The variable name is arbitrary meaning you can name it anything, but the convention is to use camelCase of the module name. This is a silly example which majority of Node developers won't use (and a great way to have your pull request rejected!):

```js
var banana = require('path')
console.log(banana.parse('/home/user/azat/program.js'))
```

Let's cover the most important core modules.

Note: the code is a simplified version from the Node.js GitHub repository where core modules are located. The file is `path.js` in the `lib` folder, in case you want to see the real implementation.

## Main Core Modules

There are a lot of core modules but not as many compare to other platforms/environments like Python. The philosophy of Node is to keep the core lean and thin. But because Node was build for networking (web app, clients and servers) with mind, the creators of Node bundled it with some great module. Really you can build your entire HTTP or IRC server with just core modules!

Here is the list of the core modules for v5.1:

* [`assert`](https://nodejs.org/api/assert.html): A simple set of assertion test
* [`cluster`](https://nodejs.org/api/cluster.html): Interface to launch a cluster (multiple) of Node processes to increase performance 
* [`crypto`](https://nodejs.org/api/crypto.html): Cryptographic functionality for OpenSSL, HMAC, etc.
* [`events`](https://nodejs.org/api/events.html): Interface for asynchronous event-driven architecture (used by many other core modules)
* [`fs`](https://nodejs.org/api/fs.html): File system I/O methods
* [`http`](https://nodejs.org/api/http.html): Interface for HTTP servers and clients
* [`net`]()
* os
* path
* querystring
* url
* utils


## fs

## path

## url

## Resources

1. []()
1. []()
1. []()


---

<a href='https://learn.co/lessons/node-modules-core' data-visibility='hidden'>View this lesson on Learn.co</a>
