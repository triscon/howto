# Install Keptn

## Install Keptn CLI

Go to [Releases](https://github.com/keptn/keptn/releases)

Take the latest stable version and copy the archive link for your operating system (we will use 0.7.1_keptn-linux.tar):

Download the archive:

```shell
wget https://github.com/keptn/keptn/releases/download/0.7.1/0.7.1_keptn-linux.tar
```

Extract files from archive: `tar xf 0.7.1_keptn-linux.tar`

Verify that the execute permission is set: `ls -l keptn`

Move binary in a `PATH` location so it's accessible: `sudo mv keptn /usr/local/bin/keptn`

Verfiy installation: `keptn version`

## Install Keptn on cluster

Prerequistite:
- We run a k3s on Ubuntu 18.04

Install Keptn (Quality Gate): `keptn install`

## Nginx ingress controller installation

If not already installed follow this installation manual: [Installation - NGINX Ingress Controller](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/)

## Configure Ingress

Create Ingress resource manifest

```yaml
# keptn-api-ingress-manifest.yaml
---
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
  namespace: keptn
  name: keptn-api-ingress
spec:
  rules:
  - host: keptn.<YOUR-SERVER-IP>.nip.io
    http:
      paths:
      - backend:
          serviceName: api-gateway-nginx
          servicePort: 80
```

Run: `kubectl -n keptn apply -f keptn-api-ingress-manifest.yaml`

## Authenticate Keptn CLI

```shell
export KEPTN_ENDPOINT=http://keptn.<YOUR-SERVER-IP>.nip.io/api
export KEPTN_API_TOKEN=$(kubectl get secret keptn-api-token -n keptn -ojsonpath='{.data.keptn-api-token}' | base64 --decode)

keptn auth --endpoint=$KEPTN_ENDPOINT --api-token=$KEPTN_API_TOKEN
```

## Keptn Bridge Credentials

Print Credentials: `keptn configure bridge --output`
