# NGINX

[NGINX](https://nginx.org) (pronounced "engine-x") is an open source reverse proxy server for HTTP, HTTPS, SMTP, POP3, and IMAP protocols, as well as a load balancer, HTTP cache, and a web server (origin server).

## TL;DR;

```bash
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install bitnami/nginx
```

Check the [Bitnami Nginx Readme](https://github.com/bitnami/charts/tree/master/bitnami/nginx)

## How to execute my helm chart

```bash
cd helmchart
helm install ./ --name react
```

## Additional Functionalities

- Added Autoscaling for the deployments

