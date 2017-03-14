# express-middleware-github-webhooks

Express middleware for handling post requests from GitHub's webhooks.

### Getting started

Install with `npm install -S express-middleware-github-webhooks`

You must ensure that the post body is parsed before using this middleware, preferably with [body-parser](https://github.com/expressjs/body-parser).

All requests are verified for authenticity. In addition, the property below is exposed for convenience:
```
req.githubWebhook = {
  headers: {
    event,
    signature,
    deliveryId,
  },
  body: req.body
};
```

### How to use

This middleware must be called with a single object argument containing the following props:
```
MANDATORY
secret {string} The secret defined in the repo's webhook settings

OPTIONAL
whitelistEvents {array} An array of strings that match github webhook events. If this argument is passed, only these events will pass through the middleware.
```

### Example

```
const expressWebhookMiddleware = require('express-middleware-github-webhooks');

const webhook = expressWebhookMiddleware({
  secret: process.env.YOUR_SECRET_HERE
});

module.exports = app => {
  // NOTE: Make sure the post body has been parsed and is available @ req.body
  app.post('/webhook', webhook, (req, res) => {
  	// req.githubWebhook is available in scope
    console.log(req.githubWebhook);
  });
};
```
