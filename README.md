# node-express-request-utils-response-cache

# [Installation](#installation)
<a name="installation"></a>

```shell
npm install @mediaxpost/request-utils-response-cache
```

# [Usage](#usage)
<a name="usage"></a>

```js
const ResponseCache = require('@dealerslink/node-express-request-utils-response-cache');
let responseCache = new ResponseCache('responses', {
  expire: 300000, // Five minutes
  onCacheHit: ((req, res, data) => {
    res.set('Content-Type', 'application/json');
  })
});

function sendResponse(req, res, next) {
  if (!res.headersSent) {
    res.set('Content-Type', 'application/json');
    for (const header in res.locals.headers) {
      if (res.locals.headers.hasOwnProperty(header)) {
        res.header(header, res.locals.headers[header]);
      }
    }
    res.status(res.locals.status);
    res.json(__.omit(res.locals.body, ['cacheExpiration']));
  }
  next();
}

// Later within the expressJS request stack
// Before other processing, check cache
app.use(responseCache.handler);

// Do other processing
// app.use...

// After other processing
app.use(responseCache.store); // This only stores when the req.needsCache is set

// Process the res.locals and send response
app.use(sendResponse);
```

See wiki for more details
