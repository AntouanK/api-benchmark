api-benchmark 
=============

A node.js tool that measures and compares performances of single and multiple apis inspired by [BenchmarkJs](http://benchmarkjs.com/)

To see an example of a request/response [look at this gist](https://gist.github.com/matteofigus/6651234)

If you want to benchmark your api via [grunt](http://gruntjs.com/) take a look at [grunt-api-benchmark](https://github.com/matteofigus/grunt-api-benchmark).

Node version: **0.8.0** required

Build status: [![Build Status](https://secure.travis-ci.org/matteofigus/api-benchmark.png?branch=master)](http://travis-ci.org/matteofigus/api-benchmark)

[![NPM](https://nodei.co/npm/api-benchmark.png?downloads=true)](https://npmjs.org/package/api-benchmark)

# Installation

```shell
npm install api-benchmark
```

# Usage

### measure(service, routes, [options, ] callback)

Measures performances of a given api for multiple routes

```js
var apiBenchmark = require('api-benchmark');

var service = { 
  server1: "http://myserver:myport/mypath/"
};

var routes = { route1: 'route1', route2: 'route2' };

apiBenchmark.measure(service, routes, function(err, results){
  console.log(results);
  // displays some stats!
});
```

### compare(services, routes, [options, ] callback)

Compares performances of a given list of api servers with the same routes. Useful in case of load balancers, globalised services, deployment of new versions.

```js
var apiBenchmark = require('api-benchmark');

var services = { 
  server1: "http://myserver:myport/mypath/",
  server2: "http://myserver2:myport2/mypath2/",
};

var routes = { route1: 'route1', route2: 'route2' };

apiBenchmark.compare(services, routes, function(err, results){
  console.log(results);
  // displays some stats, including the winner!
});
```

All the Http verbs and headers are supported.

```js
var apiBenchmark = require('api-benchmark');

var services = { 
  server1: "http://myserver:myport/mypath/",
  server2: "http://myserver2:myport2/mypath2/",
};

var routes = { 
  route1: {
    method: 'get',
    route: 'getRoute',
    headers: {
          'Cookie': 'cookieName=value',
          'Accept': 'application/json'
    }
  },
  route2: 'getRoute2',
  route3: { 
    method: 'post', 
    route: 'postRoute', 
    data: { 
      test: true, 
      moreData: 'aString' 
    }
  }
};

apiBenchmark.compare(services, routes, function(err, results){
  console.log(results);
  // displays some stats, including the winner!
});
```

### Route object

#### method
  (String, default 'get'): Http verb.

#### route
  (String): the route to benchmark

#### headers
  (Object): the headers to send

#### data
  (Object): the data sent with the request

#### expectedStatusCode
  (Number, default null): if it is a number, generates an error when the status code of the response is different

#### maxMean
  (Number, default null): if it is a number, generates an error when the mean value for a benchmark cycle is major than the expected value

#### maxSingleMean
  (Number, default null): if it is a number, generates an error when the mean across all the concurrent requests value is major than the expected value

### Options object

#### debug
  (Boolean, default false): Displays some info on the console.

#### runMode
  (String, default 'sequence'): Can be 'sequence' (each request is made after receiving the previous response) or 'parallel' (all requests are made in parallel).

#### maxConcurrentRequests
  (Number, default 100): When in runMode='parallel' it is the maximum number of concurrent requests are made.

#### delay
  (Number, default 0): When in runMode='sequence', it is the delay between test cycles (secs).

#### maxTime
  (Number, default 10): The maximum time a benchmark is allowed to run before finishing (secs).
  Note: Cycle delays aren't counted toward the maximum time.

#### minSamples
  (Number, default 20): The minimum sample size required to perform statistical analysis.

#### stopOnError
  (Boolean, default true): Stops the benchmark as soon as it receives an error. When false, the benchmark goes on and the errors are collected inside the callback.
  
# Tests

```shell
npm test
```

# Contributing

For the latest updates and release information follow [@matteofigus](https://twitter.com/matteofigus) on twitter.
Feel free to open new Issues in case of Bugs or Feature requests. 
Pull requests are welcome, possibly in new branches.

### TODO

* Command-line simple interface
* Multi-thread requests
* SOAP
* Killer mode - [ask for details](https://twitter.com/matteofigus)

# License

MIT

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/matteofigus/api-benchmark/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
