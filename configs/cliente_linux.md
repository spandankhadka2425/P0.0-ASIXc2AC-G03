
## Creando useario necessario 

```bash
sudo adduser bchecker
```

## Anadir el usuario en grupo de sudors 

```bash
 sudo usermod -aG sudo bchecker

```

## cambiar el hostname a PC-2
```bash
sudo nano /etc/hostname 
```
# Configuracion de IP

```bash
 sudo nano /etc/netplan/01-network-manager-all.yaml 
```
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp1s0:
      dhcp4: true
    enp2s0:
      addresses:
        - 192.168.INTRANET.x/24
      routes:
        - to: default
          via: 192.168.30.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
       
```

