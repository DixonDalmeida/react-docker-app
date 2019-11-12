# React-Docker-App

## Table of Contents

- [Folder Structure](#folder-structure)
- [Yarn Commands](#yarn-commands)
    - [yarn start](#yarn-start)
    - [yarn test](#yarn-test)
    - [yarn run build](#yarn-run-build)
    - [yarn test coverage](#yarn-test-coverage)
- [Local Environment Docker Compose](#local-environment---docker-compose)
    - [docker-compose build](#docker-compose-up)
    - [docker-compose ps](#docker-compose-ps)
    - [docker-compose stop](#docker-compose-stop)
- [Local Environment Monitoring](#local-environment---monitoring)
    - [CAdvisor](#cadvisor)
    - [Portainer](#portainer)
- [Dockerfile](#dockerfile)

## Folder Structure

```bash
.gitignore
docker-compose.yml
Dockerfile
Dockerfile.dev
package.json
README.md
yarn.lock
nginx/
    nginx.conf
public/
    favicon.ico
    index.html
    manifest.json
src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    registerServiceWorker.js
```

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


## Local Environment - Docker Compose

### `docker-compose build`

Build Docker image in docker-compose file using build command

`docker-compose build`

### `docker-compose up`

Up the docker containers 

`docker-compose up`

Up the docker containers in daemon mode

`docker-compose up -d`

### `docker-compose stop`

Stop the docker-compose containers

`docker-compose stop`

## Local Environment - Monitoring

### CAdvisor

Analyzes resource usage and performance characteristics of running containers. 
cAdvisor (Container Advisor) provides container users an understanding of the resource usage and performance characteristics of their running containers. It is a running daemon that collects, aggregates, processes, and exports information about running containers. Specifically, for each container it keeps resource isolation parameters, historical resource usage, and histograms of complete historical resource usage. This data is exported by container and machine-wide.
https://github.com/google/cadvisor

Check out http://localhost:8080 for the CAdvisor Dashboard

### Portainer

Portainer is a lightweight management UI which allows you to easily manage your different Docker environments (Docker hosts or Swarm clusters). Portainer is meant to be as simple to deploy as it is to use. It consists of a single container that can run on any Docker engine (can be deployed as Linux container or a Windows native container, supports other platforms too). Portainer allows you to manage all your Docker resources (containers, images, volumes, networks and more) !

Check out http://localhost:9000 for the Portainer Dashboard

## Dockerfile

Multi Stage Dockerfile is used to build the react application.
Dockerfile uses the node image to build the react application and publishes the build files to the nginx image. 
Using nginx alpine image the user will be able to naviagate the react application

```bash
FROM node:13.1.0-buster-slim as build
WORKDIR /app
ENV PATH /app/node_modules/.bin:$PATH
COPY package.json /app/package.json
RUN yarn install --silent
COPY . /app
RUN npm run build


FROM nginx:1.16.1-alpine
COPY --from=build /app/build /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

nginx.conf

```bash
server {

  listen 80;

  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
    try_files $uri $uri/ /index.html;
  }

  error_page   500 502 503 504  /50x.html;

  location = /50x.html {
    root   /usr/share/nginx/html;
  }

}
```