## Generate
https://kubernetes.io/docs/tasks/administer-cluster/certificates/

Possible tools:
- easyrsa
- openssl
- cfssl
- ...

### Openssl - Create Self-Signed CA cert 
#### Generate keys
> openssl genrsa -out ca.key 2048
#### Certificate Signing Request
> openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
#### Sign Certificates
> openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
---
### Openssl - Create Client-Cert for Admin-User
#### Generate keys
> openssl genrsa -out admin.key 2048
#### Certificate Signing Request for admin with group=system:masters
> openssl req -new -key ca.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
#### Sign Certificates
> openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt

#### Kube Api Server
Is used by a lot of different services and also users.</br>
All names have to be provided and also the used IPs.</br>
For this, an <b>openssl.cnf</b> file can be used to configure this certificate

> openssl req -new -key apiserver.key -subj "CN=kube-apiserver" -out apiserver.csr -config openssl.cnf
> openssl x509 -req -in apiserver.csr -CA ca.crt -CAkey ca.key -out apiserver.crt

```openssl.cnf
[ req ]
req_extensions = v3_req
distinguished_name = req_distinguished_name

[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudation,
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = kubernetes
DNS.2 = kubernetes.default
DNS.3 = kubernetes.default.svc
DNS.4 = kubernetes.default.svc.cluster
DNS.5 = kubernetes.default.svc.cluster.local
IP.1 = <MASTER_IP>
IP.2 = <MASTER_CLUSTER_IP>
```