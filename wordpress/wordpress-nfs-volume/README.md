

# Outline

This example describes how to scale wordpress service use nfs several types of Volumes

* [kubernetes volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

## Quickstart

```console
$ kubectl create -f nfs-server.yaml
# get nfs-server ip. 
$ kubectl describe services nfs-server | grep -E '^IP' | cut -f2 -d':' | tr -d ' '
# change wordpress.yaml PersistentVolume.spec.nfs.server value, ex: `server: 10.27.254.167`.
$ kubectl create -f mysql-secret.yaml
$ kubectl create -f mysql.yaml
$ kubectl create -f wordpress.yaml
```

## Show all resource

```console
NAME                          TYPE                                  DATA      AGE
secrets/default-token-xqztx   kubernetes.io/service-account-token   3         1h
secrets/mysql                 Opaque                                1         1h

NAME                  READY     STATUS    RESTARTS   AGE       IP           NODE
po/mysql-gqlp5        1/1       Running   0          13m       10.24.1.9    gke-garychen-default-pool-fddfb877-51mb
po/nfs-server-ccq42   1/1       Running   0          1h        10.24.2.7    gke-garychen-default-pool-fddfb877-fftg
po/wordpress-bs2dw    1/1       Running   0          1m        10.24.0.9    gke-garychen-default-pool-fddfb877-8xrl
po/wordpress-jnhln    1/1       Running   0          1m        10.24.2.10   gke-garychen-default-pool-fddfb877-fftg
po/wordpress-x4b8h    1/1       Running   0          1m        10.24.1.11   gke-garychen-default-pool-fddfb877-51mb

NAME            DESIRED   CURRENT   READY     AGE       CONTAINERS   IMAGES                      SELECTOR
rc/mysql        1         1         1         13m       mysql        mysql:5.6                   role=mysql
rc/nfs-server   1         1         1         1h        nfs-server   k8s.gcr.io/volume-nfs:0.8   role=nfs-server
rc/wordpress    3         3         3         1m        wordpress    wordpress                   app=wordpress

NAME             TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)                      AGE       SELECTOR
svc/kubernetes   ClusterIP      10.27.240.1     <none>            443/TCP                      1h        <none>
svc/mysql        LoadBalancer   10.27.245.1     130.211.242.164   3306:31146/TCP               13m       role=mysql
svc/nfs-server   ClusterIP      10.27.254.167   <none>            2049/TCP,20048/TCP,111/TCP   1h        role=nfs-server
svc/wordpress    LoadBalancer   10.27.250.255   35.229.243.229    80:32646/TCP                 1m        app=wordpress

NAME                     STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc/mysql-volumeclaim    Bound     pvc-6c110e9e-7b47-11e8-bfee-42010af0010c   95G        RWO            standard       13m
pvc/nfs-server-volume    Bound     pvc-eadb23a7-7b3e-11e8-bfee-42010af0010c   108G       RWO            standard       1h
pvc/wordpress-nfs-html   Bound     wordpress-nfs-html                         50Gi       RWX                           1m

NAME                                          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS    CLAIM                        STORAGECLASS   REASON    AGE
pv/pvc-6c110e9e-7b47-11e8-bfee-42010af0010c   95G        RWO            Delete           Bound     default/mysql-volumeclaim    standard                 13m
pv/pvc-eadb23a7-7b3e-11e8-bfee-42010af0010c   108G       RWO            Delete           Bound     default/nfs-server-volume    standard                 1h
pv/wordpress-nfs-html                         50Gi       RWX            Retain           Bound     default/wordpress-nfs-html                            1m
```