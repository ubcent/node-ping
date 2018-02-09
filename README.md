# pinguin

Just another ping wrapper for nodejs.

## Motivation

Actually, pinguin is a fork of [node-ping](https://github.com/danielzzz/node-ping). It's awesome, but maintenance of this package leaves much to be desired. Usually, they merge PRs very rarely. Moreover, this package doesn't support NodeJS standard events mechanism. For now, `pinguin` is fully equaled the source package.

## Installation

```bash
npm install pinguin
```

## Usage

Below are examples extracted from `examples`

### Tradition calls

```js
var ping = require('ping');

var hosts = ['192.168.1.1', 'google.com', 'yahoo.com'];
hosts.forEach(function(host){
    ping.sys.probe(host, function(isAlive){
        var msg = isAlive ? 'host ' + host + ' is alive' : 'host ' + host + ' is dead';
        console.log(msg);
    });
});
```

### Tradition calls with configuration

```js
var cfg = {
    timeout: 10,
    // WARNING: -i 2 may not work in other platform like window
    extra: ['-i', '2'],
};

hosts.forEach(function(host){
    ping.sys.probe(host, function(isAlive){
        var msg = isAlive ? 'host ' + host + ' is alive' : 'host ' + host + ' is dead';
        console.log(msg);
    }, cfg);
});
```

### Promise wrapper

```js
var ping = require('ping');

var hosts = ['192.168.1.1', 'google.com', 'yahoo.com'];

hosts.forEach(function (host) {
    ping.promise.probe(host)
        .then(function (res) {
            console.log(res);
        });
});
```

### Promise Wrapper with configable ping options

```js
hosts.forEach(function (host) {
    // WARNING: -i 2 argument may not work in other platform like window
    ping.promise.probe(host, {
        timeout: 10,
        extra: ['-i', '2'],
    }).then(function (res) {
        console.log(res);
    });
});
```

### Support configuration

Below is the possible configuration

```js
/**
 * Cross platform config representation
 * @typedef {Object} PingConfig
 * @property {boolean} numeric - Map IP address to hostname or not
 * @property {number} timeout - Time duration for ping command to exit
 * @property {number} min_reply - Exit after sending number of ECHO_REQUEST
 * @property {string[]} extra - Optional options does not provided
 */
```

### Output specification

* For callback based implementaiton:

```js
/**
 * Callback after probing given host
 * @callback probeCallback
 * @param {boolean} isAlive - Whether target is alive or not
 * @param {Object} error - Null if no error occurs
 */
```

* For promise based implementation

```js
/**
 * Parsed response
 * @typedef {object} PingResponse
 * @param {string} host - The input IP address or HOST
 * @param {string} numeric_host - Target IP address
 * @param {boolean} alive - True for existed host
 * @param {string} output - Raw stdout from system ping
 * @param {number} time - Time (float) in ms for first successful ping response
 * @param {Array} times - Array of Time (float) in ms for each ping response
 * @param {string} min - Minimum time for collection records
 * @param {string} max - Maximum time for collection records
 * @param {string} avg - Average time for collection records
 * @param {string} stddev - Standard deviation time for collected records
 */
```

