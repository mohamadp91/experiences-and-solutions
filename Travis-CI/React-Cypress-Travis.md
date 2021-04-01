## Integrate Cypress and travis for react testing  (production build)
```
language: node_js
node_js:
  - "10"
sudo: required

cache:
  directories:
    - ~/.cache/nodejs
    - ~/.cache/yarn
    - ~/.cache/Cypress

install:
  - yarn install --frozen-lockfile
  - yarn cypress install
  - yarn cypress verify
  - yarn global add wait-on
script:
  - yarn build
  - yarn global add serve
  - yarn serve & wait-on http://localhost:3000
  - cypress run
```

We need a package to keep React project running while `yarn serve` runs and allow Cypress to run in command line.
That package is _**wait-on**_.

### Wait-On
wait-on will wait for period of time for a file to stop growing before triggering availability which is good for 
monitoring files that are being built. Likewise wait-on will wait for period of time for other resources to remain 
available before triggering success.

For http(s) resources wait-on will check that the requests are returning 2XX (success) to HEAD or GET requests 
(after following any redirects).

wait-on can also be used in reverse mode which waits for resources to NOT be available. This is useful in waiting for 
services to shutdown before continuing.