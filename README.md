# Redis client for Node.js

## In a nutshell

- Talk to Redis from Node.js 
- Fully asynchronous; your code is called back when an operation completes
- Binary-safe; uses Node.js Buffer objects 
- Client API directly follows Redis' [command specification](http://code.google.com/p/redis/wiki/CommandReference) 
- *You have to understand how Redis works and the semantics of its command set to most effectively use this client*
- Supports Redis' new exciting PUBSUB commands

## Synopsis

    var sys = require("sys");
    var client = require("redisclient").createClient();
    client.stream.addListener("connect", function () {
        client.info(function (err, info) {
            if (err) throw new Error(err);
            sys.puts("Redis Version is: " + info.redis_version);
            client.close();
        });
    });

- Refer to the many tests in `test/test.js` for many usage examples.
- Refer to the `examples/` directory for focused examples.

## Installation

This version requires at least `Node.js v0.1.33-184-g53dd9fe` and Redis `1.3.8`.
In short, I'm trying to keep up with Node and Redis HEAD.

A [git tag](http://github.com/fictorial/redis-node-client/tree/node_0.1.33__redis_1.3.8) 
exists for Node `v0.1.33` as well.

You have a number of choices:

- git clone this repo or download a tarball and simply copy `lib/redis-client.js` into your project
- use git submodule
- use the [Kiwi](http://github.com/visionmedia/kiwi) package manager for Node.js
- use the [NPM](http://github.com/isaacs/npm) package manager for Node.js

Please let me know if the package manager "seeds" and/or metadata have issues.
Installation via Kiwi or NPM at this point isn't really possible since this repo
is depending on a unreleased version of Node.js.

## Running the tests

A good way to learn about this client is to read the test code.

To run the tests, install and run redis on the localhost on port 6379 (defaults).
Then run `node test/test.js [-v|-q]` where `-v` is for "verbose" and `-q` is for "quiet".

    $ node test/test.js
    ..................................................................
    ...........................++++++++++++++++++++++++++++++++++++

    [INFO] All tests have passed.

If you see something like "PSUBSCRIBE: unknown command" then it is time to upgrade
your Redis installation.

## Documentation

There is a method per Redis command.  E.g. `SETNX` becomes `client.setnx`.

For example, the Redis command [INCRBY](http://code.google.com/p/redis/wiki/IncrCommand)
is specified as `INCRBY key integer`.  Also, the INCRBY spec says that the reply will
be "... the new value of key after the increment or decrement."

This translates to the following client code which increments key 'foo' by 42.  If
the value at key 'foo' was 0 or non-existent, 'newValue' will take value 42 when
the callback function is called.

    client.incrby('foo', 42, function (err, newValue) {
        // ...
    });

## Notes

All commands/requests use the Redis *multi-bulk request* format which will be
the only accepted request protocol come Redis 2.0.

## Metadata

- *Author*: Brian Hammond (brian at fictorial dot com) with various patches 
  from nice people everywhere.
- *Copyright*: Copyright (C) 2010 Fictorial LLC.
- *License*: MIT

