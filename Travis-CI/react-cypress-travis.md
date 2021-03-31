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
