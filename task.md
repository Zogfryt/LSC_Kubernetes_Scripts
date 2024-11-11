## Starting minikube

For this exercise i decided to use minikube. I won't show installation of minikube, helm, docker and kubectl, since all already had this installed.
To start minikube i've created cluster with command

```
minikube start --driver=docker
```
![minikube_start](./images/img_1.png)

## Installing NFS-server

Using helm I have installed proposed NFS.
```
helm repo add nfs-ganesha-server-and-external-provisioner https://kubernetes-sigs.github.io/nfs-ganesha-server-add-external-provisioner/
helm install nfs-server nfs-ganesha-server-and-external-provisioner/nfs-server-provisioner --set=storageClass.name=storage-class
```

![helm repp add](./images/img_2.png)
![helm install](./images/img_3.png)

## Applying PVC

With created configuration file pvc.yaml (all commands has been run from home directory)

```
kubectl apply -f pvc.yaml
```
![pvc](./images/img_4.png)

## Applying deployment

With created deployment.yaml I've provisioned containers.

```
kubectl apply -f deployment.yaml
```

![deployment](./images/img_5.png)

## Applying Service

With created service.yaml I've provisioned service. I've decided to create LoadBalancer for easy access to website.

```
kubectl apply -f service.yaml
```

![svc](./images/img_6.png)

In order to access this service, i had to run tunnel in minikube.

```
minikube tunnel
```

![tunnel](./images/img_7.png)

To connect i have to collect external IP address.

```
kubectl get svc
```

![list svc](./images/img_8.png)

## Connecting before job

As I've connected PV to the nginx, folder containing website was empty.

![site_empty](./images/img_9.png)

## Applying job

With created script job.yaml i'have runned job creating and coping new html into the shared PV

```
kubectl apply -f job.yaml
```

![job](./images/img_10.png)

## Checking site

![site](./images/img_11.png)