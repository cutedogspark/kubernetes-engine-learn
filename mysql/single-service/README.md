# Outline

This example describes how to create single mysql server

## Quickstart

```console
$ kubectl create -f mysql-secret.yaml
$ kubectl create -f mysql-volumeclaim.yaml
$ kubectl create -f mysql-service.yaml
$ kubectl create -f mysql.yaml
$ kubectl describe services mysql | grep -E '^LoadBalancer' | cut -f2 -d':' | tr -d ' '
```