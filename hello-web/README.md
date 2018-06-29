# Outline

How to build container image and deploy your service.

## Quickstart

```console
$ export PROJECT_ID="$(gcloud config get-value project -q)"
# generate demo images
$ make image-v1
$ make push-image-v1
$ make image-v2
$ make push-image-v2
# show images
$ gcloud container images list-tags gcr.io/${PROJECT_ID}/hello-web
DIGEST        TAGS  TIMESTAMP
89f43ce0ad7a  v2    2018-06-29T15:14:09
5f200b12fe70  v1    2018-06-29T15:13:42
# default use image v1
$ kubectl apply -f deploy-use-image-v1.yaml
$ kubectl describe service hello-web-service | grep -E '^LoadBalancer' | cut -f2 -d':' | tr -d ' '
35.201.227.171
$ curl http://35.201.227.171/
Hello, world!
Version: 1.0.0
Hostname: hello-web-deployment-5bd96f8969-q4wp5
# update use image v2
$ kubectl apply -f deploy-use-image-v2.yaml
$ curl http://35.201.227.171/
Hello, world!
Version: 2.0.0
Hostname: hello-web-deployment-5c77fd596-p2wqc
```