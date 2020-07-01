# The Nodejs Module System 

In order to structure your program into different files, node.js provides you with a simple module system.

To illustrate the approach, let's create a new file called *'main.js'* with the following content:

```javascript
var hello = require('./hello');
hello.world();
```

As you have probably guessed, the ```require('./hello')``` is used to import the contents from another JavaScript file. The initial ```'./'``` indicates that the file is located in the same directory as *'main.js'*. Also note that you don't have to provide the file extension, as ```'.js'``` is assumed by default.

So let's go ahead and create our ```'hello.js'``` file, with the following content:

```javascript
exports.world = function() {
  console.log('Hello World');
}
```

What you notice here, is that we are assigning a property called ```'world'``` to an object called ```'exports'```. Such an ```'exports'``` object is available in every module, and it is returned whenever the require function is used to include the module. If we now go ahead and run our ```'main.js'``` program, we will see the expected output:

```bash
$ node main.js
Hello World
```

At this point it should also be mentioned that many node users are overwriting the exports object directly like so:

```javascript
module.exports = function() {
  // ...
}
```

As you might have expected, this will directly cause the require function to return the assigned function. This is useful if you're doing object oriented programming, where each file exports the constructor of one class.

The next thing you need to know about the module system is how it deals with require calls that don't include a relative hint about the location of the included file. Take for example:

```javascript
var http = require('http');
```

What node.js will do in this case, is to first look if there is a core module named http, and since that's the case, return that directly. But what about non-core modules, such as 'mysql'?

```javascript
var mysql = require('mysql');
```

In this case node.js will walk up the directory tree, moving through each parent directory in turn, checking in each to see if there is a folder called *'node_modules'*. If such a folder is found, node.js will look into this folder for a file called *'mysql.js'*. If no matching file is found and the directory root '/' is reached, node.js will give up and throw an exception.

At this point node.js also considers an additional, mutable list of alternative include directories which are accessible through the require.paths array. However, there is intense debate about removing this feature, so you are probably better off ignoring it.

Last but not least, node.js also lets you create an 'index.js' file, which indicates the main include file for a directory. So if you call ```require('./foo')```, both a *'foo.js'* file as well as an *'foo/index.js'* file will be considered, this goes for non-relative includes as well.

[source][source]

[source]:http://nodeguide.com/beginner.html#learning-javascript