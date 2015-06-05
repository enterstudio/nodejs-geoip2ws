geoip2ws
========

Unofficial Node.js module for the Maxmind GeoIP2 Web Services.

[![Circle CI](https://circleci.com/gh/fvdm/nodejs-geoip2ws/tree/master.svg?style=svg)](https://circleci.com/gh/fvdm/nodejs-geoip2ws/tree/master)


Usage
-----

There are multiple ways to load and set up this module. All communication is done over HTTPS.

#### One line

```js
require ('geoip2ws') (userId, licenseKey) (ip, callback);
```

#### Multiple calls, same account

```js
var geo = require ('geoip2ws') (userId, licenseKey);
geo (ip1, callback);
geo (ip2, callback);
```

#### Mix products (or accounts)

```js
var insights = new require ('geoip2ws') (userId, licenseKey, 'insights');
var country = new require ('geoip2ws') (userId, licenseKey, 'country');

// precise lookup, higher cost
insights (ip, callback);

// vague lookup, lower cost
country (ip, callback);
```


Installation
------------

You need a Maxmind account ID and license key with enough credits for one of their GeoIP *web*
services. You can find both [*here*](https://www.maxmind.com/en/my_license_key).

`npm install geoip2ws`


The functions
-------------

The _first function_ is the setup and takes these settings:

parameter      | type    | required | description
---------------|---------|----------|---------------------------------
userId         | string  | yes      | Your Maxmind account user ID
licenseKey     | string  | yes      | Your Maxmind account license key
service        | string  | no       | The service you'd like to use: `insights`, `country`, `city` (default)
requestTimeout | integer | no       | Socket read timeout in milliseconds to wait for reply from MaxMind

```js
// With arguments
var geo = require ('geoip2ws') (1234, 'abc', 'country', 2000);

// Or with an object
var geo = require ('geoip2ws') ({
  userId: 1234,
  licenseKey: 'abc',
  service: 'country',
  requestTimeout: 2000
});
```


The _second function_ does the IP-address lookup and takes these arguments:

parameter | type     | required | description
----------|----------|----------|-----------------------------------
service   | string   | no       | The service, same as above
ip        | string   | yes      | The IPv4 or IPv6 address to lookup
callback  | function | yes      | Your callback `function` to receive the data

```js
geo ('city', '145.53.252.135', myCallback);
```


Callback
--------

The callback function receives result data and errors. Unless an error occurs the
data JSON will be parsed to an object. When everything is ok `err` is `null` else
`err` is an instance of `Error`. It also returns API errors this same way.

```js
function myCallback (err, data) {
  if (err) {
    console.log (err);
  } else {
    console.log (data.city.names.en);
  }
}
```

Response data: <http://dev.maxmind.com/geoip/geoip2/web-services/#Response_Body>


#### Errors

error message           | description                      | additional
------------------------|----------------------------------|---------------------
no userId or licenseKey | You did not set your credentials |
invalid service         | The service name is invalid      | no credits used
invalid ip              | The IP-address is invalid        | no credits used
request failed          | A request error occured          | `err.error`
API error               | API error occured                | `err.code` and `err.error`


API errors: <http://dev.maxmind.com/geoip/geoip2/web-services/#Errors>


Development
-----------

The source code repository has two main branches: `master` and `develop`.
The `master` is exactly the same as the latest release on [npm](https://www.npmjs.com/package/geoip2ws),
while `develop` is where all development happens and other branches and Pull Requests are merged into.

When you intent to submit a Pull Request, please code against the `develop` branch.


* Branch status: [![Circle CI](https://circleci.com/gh/fvdm/nodejs-geoip2ws/tree/develop.svg?style=svg)](https://circleci.com/gh/fvdm/nodejs-geoip2ws/tree/develop)
* [Source code](https://github.com/fvdm/nodejs-geoip2ws)
* [Contribution Guide](https://guides.github.com/activities/contributing-to-open-source/#contributing)


### Testing

You can test your code with `npm test` in the source path or simply create a PR and see what happens.

* With account the API endpoint is at `https://geoip.maxmind.com`: credits count, real data.
* Without account the API endpoint is at `https://frankl.in`: no credits, no logs, fake data.


#### Test configuration

variable          | description                   | default
------------------|-------------------------------|----------------------------------------
GEOIP2WS_USERID   | Account user ID               | *null*
GEOIP2WS_LICENCE  | Account license key           | *null*
GEOIP2WS_SERVICE  | `country`, `city`, `insights` | city
GEOIP2WS_TIMEOUT  | Wait time out in ms           | 5000
GEOIP2WS_ENDPOINT | API endpoint, i.e. http proxy | `https://geoip.maxmind.com/geoip/v2.1/`


Example: `GEOIP2WS_TIMEOUT=30000 npm test`


Unlicense
---------

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>


Author
------

Franklin van de Meent
| [Website](https://frankl.in/)
| [Github](https://github.com/fvdm)
