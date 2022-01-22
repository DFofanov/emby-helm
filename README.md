# emby-helm
A simple deployment for emby on a single node local machine

## Usage

Modify the values.yaml to match your servers environment, ensure the paths defined actually exist on the host and have sufficient space, ensure kubernetes is installed and you can access it, and it's the correct environment, and then simply run:

``` cmd
helm install -f values.yaml media ./
```

## Installing Cert-manager

``` shell
# kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.crds.yaml
# kubectl create namespace cert-manager
# helm repo add jetstack https://charts.jetstack.io
# helm repo update
# helm install cert-manager --namespace cert-manager --version v1.6.1 jetstack/cert-manager
```
An example of nginx Ingress with automatic generation of a SSL/TLS certificate

``` yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt
  name: emby-web-ingress
  namespace: media
spec:
  rules:
  - host: "{{ .Values.FQDN }}"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: emby-service
            port:
              number: 8096
    tls:
    - hosts:
      - "{{ .Values.FQDN }}"
      secretName: socks-tls-example
```

## License
Licensed under the GPL-3.0 License.