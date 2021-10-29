# Kubernetes

- [Kubernetes](#kubernetes)
  - [Prerequisites](#prerequisites)
  - [Deployment](#deployment)

This README will document the steps required to deploy project-megaphone applications using Kubernetes.

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- a connection to an existing and functiona k8s cluster
- [helm](https://helm.sh/)

## Deployment

1. Create the apps namespace

   ```bash
   kubectl create namespace apps
   ```

1. Install [ingress-nginx](https://kubernetes.github.io/ingress-nginx/)

   ```bash
   helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
   helm repo update

   kubectl create namespace ingress-nginx
   helm install -n ingress-nginx ingress-nginx ingress-nginx/ingress-nginx
   ```

   To make sure the ingress controller was created properly, you can search for services under the `nginx-ingress` namespace:

   ```bash
   kubectl get service -n ingress-nginx
   ```

   You should see a service with the name `ingress-nginx-controller` appear with an external IP:

   ```plaintext
   NAME                       TYPE           CLUSTER-IP        EXTERNAL-IP       PORT(S)                      AGE
   ingress-nginx-controller   LoadBalancer   XXX.XXX.XXX.XXX   XXX.XXX.XXX.XXX   80:XXXXX/TCP,443:XXXXX/TCP   62m
   ```

1. Install cert-manager

   ```bash
   kubectl apply -f https://github.com/jetstack/cert-manager/releases/latest/download/cert-manager.yaml
   ```

1. Deploy resources

   ```bash
   kubectl apply -f manifests/
   ```
