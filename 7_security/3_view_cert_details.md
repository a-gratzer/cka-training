https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/tools/kubernetes-certs-checker.xlsx
## Inspect logs
> journalctl -u etcd.service -l <br>
> kubectl logs etcd-master <br>
> docker ps -a <br>
> docker logs ....


## Inspect admin.crt
> openssl x509 -in example/admin.crt -text -noout

```terminal

Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number:
            16:6c:ad:15:da:0d:d7:38:11:0b:54:87:c3:94:66:54:a7:16:d3:6f
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: CN = KUBERNETES-CA
        Validity
            Not Before: Aug 16 15:40:06 2023 GMT
            Not After : Sep 15 15:40:06 2023 GMT
        Subject: CN = kube-admin, O = system:masters
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:a2:31:be:cd:27:b3:da:24:6b:f8:1d:5b:03:a8:
                    f7:bc:d0:a3:65:30:3d:2f:ee:44:fc:92:d3:7c:8f:
                    be:76:4f:4b:59:ff:b2:1e:44:35:f4:e9:63:dc:93:
                    3a:17:91:28:a9:c3:4b:9c:ba:a2:44:6e:fa:e8:c8:
                    b8:f1:30:aa:be:63:18:59:07:3b:9e:6f:c1:1f:d6:
                    b7:68:f6:0e:df:06:9f:2f:17:32:c0:54:d9:1e:19:
                    07:9d:9f:41:b4:fb:7d:a1:41:22:e0:a3:30:30:76:
                    e4:0b:04:99:5e:61:ad:91:d2:29:d9:92:de:d2:82:
                    97:cc:cd:29:08:d7:f2:ad:af:41:fe:66:42:6f:08:
                    dc:c8:9a:45:85:54:c3:38:b7:3f:47:a3:8a:08:43:
                    10:f9:7e:ea:6c:45:f4:0c:10:03:c6:d0:42:fa:e7:
                    c7:e6:d0:e8:ec:33:3b:76:c4:d3:34:8e:25:4b:12:
                    33:d5:40:71:f6:ac:77:67:34:08:9a:53:cb:b4:da:
                    d5:6c:22:4d:c8:a3:19:70:a8:40:f8:40:5d:89:39:
                    a0:f9:18:08:96:91:94:c5:01:99:d6:bd:15:8d:d9:
                    7e:d5:cb:fe:f3:77:c0:a9:1f:b1:9d:57:25:0d:cb:
                    48:f4:82:48:13:6c:c0:06:d9:8f:e5:18:6f:91:eb:
                    89:cd
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption
    Signature Value:
        0a:39:24:2f:31:46:fc:b7:ae:1c:4d:d7:a7:6f:6a:b8:31:d2:
        71:f8:56:04:4b:2c:bd:64:1d:43:05:db:83:0e:3a:fe:df:bd:
        df:79:08:0e:5b:50:39:d9:36:d8:8a:30:2e:ad:c1:31:89:b5:
        ea:83:eb:ef:0d:4e:3f:7d:3c:9c:16:1d:34:43:31:6a:cf:5c:
        2d:2b:1d:9f:77:ff:ea:05:ac:73:9c:2e:4f:ea:1f:85:18:ea:
        87:3b:ef:49:d6:30:a5:9a:5a:f8:ac:f6:26:3d:23:5c:93:83:
        75:4c:cf:a8:1f:7c:2a:30:91:fb:c2:98:f2:61:97:d2:4d:a9:
        85:b5:62:14:1d:55:90:49:e6:de:b9:04:cd:dc:73:3d:cd:62:
        8b:c4:ad:aa:53:0d:b2:32:da:81:af:ae:f5:84:d3:15:2b:94:
        b0:2d:c3:b6:ec:bf:75:4b:f1:91:8e:50:c2:af:f5:0d:f5:aa:
        ea:2a:bf:7a:46:bf:ca:58:e1:f1:a6:89:d4:9d:6d:b4:43:34:
        ff:a6:2b:d7:5d:4e:36:f7:43:f8:cd:d7:c6:ac:78:a1:c2:40:
        d2:a8:5a:8a:2c:1d:b1:f1:4f:60:34:a8:3f:3c:1b:85:45:c1:
```