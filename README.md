[![NPM version](https://badge.fury.io/js/node-modbus.png)](http://badge.fury.io/js/node-modbus)
[![Build Status](https://travis-ci.org/biancode/node-modbus.svg?branch=master)](https://travis-ci.org/biancode/node-modbus)
[![Standard - JavaScript Style Guide](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)
[![NPM download](https://img.shields.io/npm/dm/node-modbus.svg)](http://www.npm-stats.com/~packages/node-modbus)

[![nodemodbus64](images/modbus-icon64.png)](https://www.npmjs.com/package/node-red-contrib-modbus)

node-modbus
===========

NODE-MODBUS is a simple Modbus TCP/Serial Client/Server API.

# Install

Run the following command in the root directory of your Node-RED install

    npm install node-modbus

Run the following command for global install

    npm install -g node-modbus

Testing
-------

The test files are implemented using [mocha](https://github.com/visionmedia/mocha) and sinon.
Simply use `npm-update.sh` in the source code project.
To run the tests type from the projects root folder `mocha test/*`.
Please feel free to fork and add your own tests.

Examples
--------

### Server TCP
```js
let node_modbus = require('node-modbus')
let server = node_modbus.server.tcp.complete({ port : 502, responseDelay: 200 })
```

### Client TCP
```js
const node_modbus = require('node-modbus')

const client = node_modbus.client.tcp.complete({
    'host': 'modbus.server.local', /* IP or name of server host */
    'port': 502, /* well known Modbus port */
    'unitId': 1, 
    'timeout': 2000, /* 2 sec */
    'autoReconnect': true, /* reconnect on connection is lost */
    'reconnectTimeout': 15000, /* wait 15 sec if auto reconnect fails to often */
    'logLabel' : 'ModbusClientTCP', /* label to identify in log files */
    'logLevel': 'debug', /* for less log use: info, warn or error */
    'logEnabled': true
})

const time_interval = 1000
client.connect()
client.on('connect', function () {
  setInterval( function () {
     client.readCoils(0, 13).then((response) => console.log(response.payload))
  }, time_interval) /* reading coils every second */
})
```

### Server Serial

TBD

### Client Serial
```js
const node_modbus = require('node-modbus')

const client = modbus.client.serial.complete({
    'portName': '/dev/ttyS0', /* COM1 */
    'baudRate': 9600, /* */
    'dataBits': 8, /* 5, 6, 7 */
    'stopBits': 1, /* 1.5, 2 */
    'parity': 'none', /* even, odd, mark, space */
    'connectionType': 'RTU', /* RTU or ASCII */
    'connectionDelay': 250, /* 250 msec - sometimes you need more on windows */
    'timeout': 2000, /* 2 sec */
    'autoReconnect': true, /* reconnect on connection is lost */
    'reconnectTimeout': 15000, /* wait 15 sec if auto reconnect fails to often */
    'logLabel' : 'ModbusClientSerial', /* label to identify in log files */
    'logLevel': 'debug', /* for less log use: info, warn or error */
    'logEnabled': true
})

/* here we need none connect call */

const time_interval = 1000
client.on('connect', function () {
  setInterval( function () {
     client.readCoils(0, 13).then((response) => console.log(response.payload))
  }, time_interval) /* reading coils every second */
})
```

## License

[MIT](LICENSE)

Based on [jsmodbus][1]

[1]:https://github.com/Cloud-Automation/node-modbus
[2]:https://github.com/visionmedia/mocha
