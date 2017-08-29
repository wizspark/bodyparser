@wize/koa-bodyparser
===============

A body parser for koa, base on [co-body](https://github.com/tj/co-body).

## Install

[![NPM](https://nodei.co/npm/koa-bodyparser.png?downloads=true)](https://nodei.co/npm/koa-bodyparser/)

## Usage

``` js
var bodyParser = require('koa-bodyparser');
var Koa = require('koa');

var app = new Koa();
app.use(bodyParser());

app.use(ctx => {
  // the parsed body will store in this.request.body
  // if nothing was parsed, body will be an empty object {}
  ctx.body = ctx.request.body;
});
```

## Options

* **enableTypes**: parser will only parse when request type hits enableTypes, default is `['json', 'form']`.
* **encode**: requested encoding. Default is `utf-8` by `co-body`.
* **formLimit**: limit of the `urlencoded` body. If the body ends up being larger than this limit, a 413 error code is returned. Default is `56kb`.
* **jsonLimit**: limit of the `json` body. Default is `1mb`.
* * **strict**: when set to true, JSON parser will only accept arrays and objects. Default is `true`. See [strict mode](https://github.com/cojs/co-body#options) in `co-body`. In strict mode, `this.request.body` will always be an object(or array), this avoid lots of type judging. But text body will always return string type.
* **detectJSON**: custom json request detect function. Default is `null`.

  ```js
  app.use(bodyparser({
    detectJSON: function (ctx) {
      return /\.json$/i.test(ctx.path);
    }
  }));
  ```

* **extendTypes**: support extend types:

  ```js
  app.use(bodyparser({
    extendTypes: {
      json: ['application/x-javascript'] // will parse application/x-javascript type body as a JSON string
    }
  }));
  ```

* **onerror**: support custom error handle, if `koa-bodyparser` throw an error, you can customize the response like:

  ```js
  app.use(bodyparser({
    onerror: function (err, ctx) {
      ctx.throw('body parse error', 422);
    }
  }));
  ```

## Licences

[MIT](LICENSE)
