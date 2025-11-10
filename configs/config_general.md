##configuracion IP 

# servers

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
       dhcp4: true
    enp2s0:
      addresses:
        -  192.168.x.x/24
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

```
# Client 

```

```