# Irrelon SyncScript
> This is beta code and should not be relied on for production use yet. It may not work in all cases and
may generate incorrect output from your code. When it is production ready we will update this message.

Write clean, readable synchronous JavaScript code and output to callback-based async code automatically.
Compiles to readable, compatible JS code that runs everywhere and on every browser. No libraries or
dependencies, just clean code.

SyncScript is compatible with "normal" JavaScript and you can still write callback-style code if you want
to. All features and functionality of JavaScript is still available and left unaffected by SyncScript. All
existing libraries are compatible and will remain unaffected by the compilation process.

You can still interface with standard callback-based code and libraries as usual or by using the SyncScript
sync() method to call an async method synchronously.

## Why?
If you've coded in JavaScript for any length of time you will know that pretty soon you start to
pile up tons of callbacks (callback-hell as it is sometimes referred). You hate it, I hate it and
so does everyone else that codes in JS but it is a necessary evil... or is it?

## Synchronous Code in an Async World
Imagine that instead of writing:

```
getHtmlFromUrl('http://www.myserver.org/someHtmlFile.html', function (err, response, body) {
  if (!err && response.status === 200) {
    $('body').append(body);
  }
});
```

You could just write:

```
var err, response, body = sync(getHtmlFromUrl('http://www.myserver.org/someHtmlFile.html'));

if (!err && response.status === 200) {
  $('body').append(body);
}
```

Notice how we threw away the callback function and just used the err, repsonse and body variables as if they were immediately
defined? This is how Irrelon SyncScript looks. You'll also notice that when an async method has multiple
return values we simply define multiple variables to collect them:

```
var err, response, body = sync(getHtmlFromUrl('http://www.myserver.org/someHtmlFile.html'));
```

Let's look at another example where you want to do a lot of calls where each is async and you need to wait for each one
to complete before executing the next (waterfall pattern):

```
asyncCall(null, function (err, data1) {
  if (!err) {
    asyncCall(data1, function (err, data2) {
      if (!err) {
        asyncCall(data2, function (err, data3) {
          if (!err)  {
            // Yay we have our final data in data3
          }
        });
      }
    });
  }
});
```

Now look at how you would write that in SyncScript:

```
var err, data1 = sync(asyncCall(null));
if (!err) { var err, data2 = sync(asyncCall(data1)); }
if (!err) {
  var err, data3 = sync(asyncCall(data2));
  // Yay we have our final data in data3
}
```

Synchronous code is easier to read and understand and requires less boilerplate effort and more of what actually makes
your application work.

## How to Use It

### Installation via NPM
Install SyncScript via npm:

	npm install -g syncscript

### Use From Command Line
To compile a SyncScript file to JavaScript you can use the command line:

	syncscript <yourinputfile.js> <youroutputfile.js>

E.g.

	syncscript ./test/guineaPig.js ./test/output.js

If you leave off the second argument the output is sent to stdout which allows you to pipe it elsewhere.

### Use From JetBrains IDE via File Watcher
File watchers auto-execute programs against your code as you make changes. A good example would be Google's
closure compiler which you can set up in WebStorm or PHPStorm to auto-compile and minify your JavaScript
code as you make changes.

Since SyncScript's command-line interface supports both file-based output and stdout you can use a file
watcher to compile your SyncScript code on the fly as you make changes into a normal JavaScript file.

Further instructions and screenshots coming shortly...

### Using Compiler in Your Own Node.js Projects
This section is being updated, please hang tight! SyncScript works by compiling your source code to standard JavaScript
callbacks and requires Node.js to do the compilation part. Once the output file is generated it can run on all browsers
and Node.js as if you wrote the callback-hell version yourself.

In this section I will describe how to use SyncScript as a compiler from the command line, as a file-watcher in JetBrains
IDEs such as WebStorm and PHPStorm and also how to include it in your own Node.js applications so you can take the compiled
output and do further processing to the code.

### Online Compiler Service
Instead of installing SyncScript you can send your code to the online compiler service or paste it into the
compiler page to instantly see the compiler output.

Further instructions for the online compiler service coming shortly...

## Copyright and License
SyncScript is copyright Irrelon Software Limited and license under the open-source MIT license.

## Contributing, Issues and Bugs
Contributions and pull requests are welcome! If you have any issues or find any bugs please log them in the GitHub issue 
tracker for this repo.
