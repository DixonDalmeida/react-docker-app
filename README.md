# Sample-React-Docker-App

## Table of Contents

- [Folder Structure](#folder-structure)
- [Yarn Commands](#yarn-commands)
  - [yarn start](#yarn-start)
  - [yarn test](#yarn-test)
  - [yarn run build](#yarn-run-build)
  - [yarn test coverage](#yarn-test-coverage)

## Folder Structure

│   .gitignore
│   docker-compose.yml
│   Dockerfile
│   Dockerfile.dev
│   package.json
│   README.md
│   yarn.lock
│
├───nginx
│       nginx.conf
│
├───public
│       favicon.ico
│       index.html
│       manifest.json
│
└───src
        App.css
        App.js
        App.test.js
        index.css
        index.js
        logo.svg
        registerServiceWorker.js

## Yarn Commands

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br>
You will also see any lint errors in the console.

### `yarn test`

Launches the test runner in the interactive watch mode.<br>
See the section about [running tests](#running-tests) for more information.

### `yarn run build`

Builds the app for production to the `build` folder.<br>
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br>
Your app is ready to be deployed!


### `yarn test coverage`
Jest has an integrated coverage reporter that works well with ES6 and requires no configuration.
Run yarn test -- --coverage

