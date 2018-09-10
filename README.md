# Passport-Stack-Exchange

[Passport](https://github.com/jaredhanson/passport) strategy for authenticating with [Stack Exchange](https://stackexchange.com/) using the OAuth 2.0 API.

This module lets you authenticate using Stack Exchange in your Node.js applications. By plugging into Passport, Stackexchange authentication can be easily and unobtrusively integrated into any application or framework that supports [Connect](http://www.senchalabs.org/connect/)-style middleware, including [Express](http://expressjs.com/).

## Install

```bash
npm install passport-stack-exchange
```

## Usage

#### Configure Strategy

The Stack Exchange authentication strategy authenticates users using a Stack Exchange account (Stack Overflow) and OAuth tokens.  The strategy requires a `verify` callback, which accepts these credentials and calls `done` providing a user, as well as `options` specifying a consumer key, consumer secret, callback URL, and API key for [Stackapps](https://stackapps.com). Register the application on the [service page](https://stackapps.com/apps/oauth/register).

```javascript
var StackExchangeStrategy = require('passport-stack-exchange');

passport.use(new StackExchangeStrategy({
    clientID: STACKEXCHANGE_CLIENT_ID,
    clientSecret: STACKEXCHANGE_CLIENT_SECRET,
    callbackURL: 'http://127.0.0.1:3000/auth/stack-exchange/callback',
    stackAppsKey: STACKEXCHANGE_APPS_KEY,
    site: 'stackoverflow'
  },
  function(accessToken, refreshToken, profile, done) {
    User.findOrCreate({ stackexchangeId: profile.username }, function (err, user) {
      return done(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'stack-exchange'` strategy, to authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/) application:

```javascript
app.get('/auth/stack-exchange',
  passport.authenticate('stack-exchange'));

app.get('/auth/stack-exchange/callback',
  passport.authenticate('stack-exchange', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
```

## Examples

For a complete, working example, refer to the [login example](https://github.com/acruxray/passport-stack-exchange/tree/master/examples).

## Credits

* [Moo Yeol, Lee (Prescott)](http://github.com/mooyoul)
* [Jared Hanson](http://github.com/jaredhanson)

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2015 Moo Yeol, Lee (Prescott)
