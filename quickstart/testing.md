# Testing

## Information

Please make sure that all your changes works on [Chrome](https://www.google.de/chrome/browser/desktop/index.html) , [Firefox](https://www.mozilla.org/de/firefox/new/) and [Safari](https://www.apple.com/de/safari/)!  
For html/css components please check [caniuse](https://caniuse.com/).

1. Set the password for the demo user `schueler@schul-cloud.org`
   1. Ubuntu/Mac: `export SC_DEMO_USER_PASSWORD={PASSWORD}` \(Without braces\)
   2. Windows: `set SC_DEMO_USER_PASSWORD={PASSWORD}` \(Without braces\)
2. run `npm run test`
3. If you want to use another backend url than localhost, set the `BACKEND_URL` and `PUBLIC_BACKEND_URL` environment variables \(see 1\)
4. If you want to list the coverage, run `npm run coverage`

## Frontend Testing

We are currently using [nightwatch.js](http://nightwatchjs.org/) for frontend testing. The current api documentation can be found [here](http://nightwatchjs.org/api).

1. Start a client server with `npm run watch`
2. Open another command line and type `npm run frontendTests` to run tests against chrome and firefox.

In case you want browser specific tests use: `./node_modules/.bin/nightwatch --config nightwatch.conf.js --env chrome` or switch chrome with firefox.

Adding new tests:

1. Copy any of the existing tests
2. First test cases should be essentially the same, login, checkups, ...
3. Add your test cases in between the checkups and the `browser.end()`

Add your test to `diff.sh`: `diff.sh` compares the PR Branch with the Master Branch and then adds the tests in case any files where changed for which a test exists.

