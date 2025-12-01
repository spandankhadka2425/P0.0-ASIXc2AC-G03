
---
# Configuración de IP

A continuación se muestra cómo estaba la configuración de red **antes** y cómo debe quedar **después** de la configuración correcta en el archivo `/etc/netplan/00-installer-config.yaml`.

---

### Antes de la configuración

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
       dhcp4: true
    enp2s0:
      addresses:
        - 192.168.INTRANET.x/24
      routes:
        - to: default
          via: 192.168.130.1  
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

---

### Después de la configuración

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
      optional: true
    enp2s0:
      dhcp4: false
      addresses: [192.168.30.47/24] #INTRANET
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
      routes:
        - to: 192.168.130.0/24
          via: 192.168.30.1
    enp3s0:
      dhcp4: false
      addresses: [192.168.130.47/24] #DMZ
```
> Se ha añadido una nueva interfaz `enp3s0` para que el servidor web y el servidor de base de datos (BBDD) puedan tener conectividad directa entre sí, facilitando la comunicación interna y mejorando el rendimiento de los servicios.
---

**Explicación:**  
La configuración final define correctamente las IPs estáticas para las interfaces de la Intranet y la DMZ, los servidores DNS y las rutas necesarias para la comunicación

## 1. Instalación de Servicios

```bash
sudo apt install isc-dhcp-server
sudo apt install bind9
```

## 2. Backup de Configuraciones

```bash
sudo mkdir /etc/bind/backup
sudo cp /etc/bind/named.conf /etc/bind/backup/named.conf.backup
sudo cp /etc/bind/named.conf.options /etc/bind/backup/named.conf.options.backup
sudo cp /etc/bind/named.conf.local /etc/bind/backup/named.conf.local.backup
sudo cp /etc/bind/named.conf.default-zones /etc/bind/backup/named.conf.default-zones.backup
```

## 3. Aplicar Configuración de Red

```bash
sudo netplan apply
```

## 4. Verificación de Red

```bash
ip route show
```

## 5. Pruebas de Conectividad

```bash
curl http://localhost/CSV.php
curl http://192.168.30.47/CSV.php
```

## 6. Diagnóstico de Servicios

```bash
sudo netstat -tulpn | grep :80
sudo ufw status
```

## 7. Configuración de Firewall

```bash
sudo ufw allow 80/tcp
```

## 8. Configuración de Apache

```bash
sudo nano /etc/apache2/ports.conf
# Contenido correcto de ports.conf:
# Listen 192.168.30.47:80
# Listen 192.168.130.47:80

#192.168.30.47:80 → Para la red Intranet (clientes internos)
#192.168.130.47:80 → Para la red DMZ (servidor web y acceso externo)

sudo systemctl restart apache2
```

## 9. Verificación de Configuración Apache

```bash
sudo grep -r "Listen" /etc/apache2/
sudo apache2ctl configtest
sudo apache2ctl -S
sudo grep -r "VirtualHost" /etc/apache2/sites-enabled/
```

## 10. Reinicio Completo de Apache

```bash
sudo systemctl stop apache2
sudo pkill -f apache2
sudo systemctl start apache2
```

## 11. Verificación Final

```bash
sudo netstat -tulpn | grep :80
# Debería mostrar:
# tcp        0      0 192.168.30.47:80        0.0.0.0:*               LISTEN      [PID]/apache2        
# tcp        0      0 192.168.130.47:80       0.0.0.0:*               LISTEN      [PID]/apache2

```
---
