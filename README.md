# Main Core Modules

## Overview

This lesson will cover the usage of the core Node modules. Also, we'll list the main core modules which will be useful during this course.

## Objectives

1. Provide a list of main core modules and their purpose
1. Describe fs and its main methods
1. Describe url and its main methods
1. Describe path and path.join

## Core Modules

As discussed previously, Node is a platform or an environment in which we build applications. You saw that modularized code is better and how you can write your own modules. With modules, developers from different teams and different companies can share their code. This is great but how do you get the code? Imagine a simple thing like parsing a path, i.e., we have a string like `/home/user/azat/program.js` and we need to extract the extension, base, file name, etc.

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

The variable name is arbitrary, but the convention is to use camelCase of the module name. This is a silly example which the majority of Node developers won't use (and is a great way to have your pull request rejected!):

```js
var banana = require('path')
console.log(banana.parse('/home/user/azat/program.js'))
```

Let's cover the most important core modules.

Note: the code is a simplified version from the Node.js GitHub repository where core modules are located. The file is `path.js` in the `lib` folder, in case you want to see the real implementation.

## Main Core Modules

There are a lot of core modules but not as many compare to other platforms/environments like Python. The philosophy of Node is to keep the core lean and thin. But because Node was build for networking (web app, clients and servers) with mind, the creators of Node bundled it with some great modules. Really you can build your entire HTTP or IRC server with just core modules!

Here is the list of the core modules for v5.1:

* [`assert`](https://nodejs.org/api/assert.html): A simple set of assertion tests
* [`cluster`](https://nodejs.org/api/cluster.html): Interface to launch a cluster (multiple) of Node processes to increase performance 
* [`crypto`](https://nodejs.org/api/crypto.html): Cryptographic functionality for OpenSSL, HMAC, etc.
* [`events`](https://nodejs.org/api/events.html): Interface for asynchronous event-driven architecture (used by many other core modules)
* [`fs`](https://nodejs.org/api/fs.html): File system I/O methods
* [`http`](https://nodejs.org/api/http.html): Interface for HTTP servers and clients
* [`net`](https://nodejs.org/api/net.html): Interface for networking which can be used to implement any protocol, not just HTTP (`http` uses `net`)
* [`os`](https://nodejs.org/api/os.html): Basic operating-system utilities
* [`path`](https://nodejs.org/api/path.html): Utilities for handling and transmitting file paths on various platforms.
* [`querystring`](https://nodejs.org/api/querystring.html): Utilities for working with query strings (e.g., `?key1=value1&key2=value2` in a URL)
* [`stream`](https://nodejs.org/api/stream.html): Abstract interface implemented by various objects.
* [`url`](https://nodejs.org/api/url.html): Utilities for URL resolution and parsing.
* [`util`](https://nodejs.org/api/util.html): Utilities which are mostly used by other core Node modules

Are your head is about to explode from this list of core modules? Don't panic! You don't need to remember all of them. Just know that they are out there when you have a task at hand which requires you to require that module (pun intended), e.g., `crypto` for hashing of the passwords.

In the next section, we'll show you some examples of `fs`, `path`, and `url`.

## fs

You can read a file asynchronously with `fs.readFile` and write to a file with `fs.writeFile`, e.g.,

```js
var callback = function(error, data) {
  console.log(data, error)
}
fs.readFile('data.json', 'utf8', callback)

fs.writeFile('message.txt', 'Hello Node.js', function (error) {
  console.log(error)
})
```

## path

We all want to develop applications that work across platform with minimal or no code modifications, because then we gain a broader user base and flexibility. Node works out of the box with Unix, Linux, Mac OS X and Windows systems.

But what would happen if I run a code with a Unix-like path on a Windows machine?

```js
fs.readFile('configurations/data/models/user.json', 'utf8', callback)
```

It probably won't do anything. So we need to have if/else conditions in our code:

```js
var platform = require('os').platform()

if (platform == 'win32') {
  var filePath = 'configurations\\data\\models\\'
} else { // assume platform is darwin or similar
  var filePath  = 'configurations/data/models/'
}
fs.readFile(filePath + 'user.json', 'utf8', callback)
```

Why don't we refactor our example and use the `join()` interface from `path`? The method will take unlimited number of arguments and return a proper path where each argument is a separated by the right delimiter (`/` or `\` depending on the OS):

```js
var path = require('path')
fs.readFile(path.join('configurations', 'data', 'models', 'user.json'), 'utf8', callback)
```


## url

URL strings can contain a lot of information for example the protocol name, domain name, port number, authentication info, path, query string, etc. Obviously, we don't want to parse this information manually. Creating your own module is also a not a great idea, because there's one in the core already!

Consider this example:

```js
url.parse('https://nodejs.org/api/url.html#url_url_parsing')
```

The result of the expressions will be this:

```
{
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'nodejs.org',
  port: null,
  hostname: 'nodejs.org',
  hash: '#url_url_parsing',
  search: null,
  query: null,
  pathname: '/api/url.html',
  path: '/api/url.html',
  href: 'https://nodejs.org/api/url.html#url_url_parsing' 
}
```

## Resources

1. [Official documentation](https://nodejs.org/api/index.html)
1. [Node.js Tutorial for Beginners - 8 - fs module video](https://www.youtube.com/watch?v=GdBgP71CSow)
1. [Node GitHub repository](https://github.com/nodejs/node/tree/master/lib)


---

<a href='https://learn.co/lessons/node-modules-core' data-visibility='hidden'>View this lesson on Learn.co</a>
