### Make CLS play nice with Redis

Forked from [`cls-redis`](https://github.com/othiym23/cls-redis)

[`redis`](github.com/NodeRedis/node_redis) works great, unless
you're using cls or cls-hooked, which wants to provide consistent access to stored values
across entire asynchronous call chains. You could use `ns.bind` to put all your
Redis callbacks on the correct continuation chain, but that breaks down if you
forget even one callback passed to `client.get()`.

This shim's job is to take care of the bookkeeping for you. It monkeypatches
the Redis driver to ensure that all the callbacks you provide are bound to the
CLS namespace you provide to the shim. Use it like so:

```js
var cls = require('continuation-local-storage'); // var cls = require('cls-hooked');
var ns = cls.createNamespace('test');

var patchRedis = require('cls-redis-patch');
patchRedis(ns);

var redis = require('redis');
var client = redis.createClient();
```

You can patch Redis for more than one namespace, but you're going to notice the
performance impact pretty quickly, so try not to do that.

### Redis Support
Tested on redis v2.6.0

### Tests

The tests assume a Redis server is up and running on localhost on the standard
port.

### TODO
Support latest version of [`cls-redis`](https://github.com/othiym23/cls-redis)
