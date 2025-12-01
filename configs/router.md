# 📋 COMANDOS DE CONFIGURACIÓN DEL ROUTER R-N03

A continuación se detallan los comandos y configuraciones necesarios para desplegar y verificar el router R-N03 en el proyecto. Cada sección incluye una explicación en español sobre su función.

---

## 1. CONFIGURACIÓN DE RED (NETPLAN)

```bash
# Editar configuración de red
sudo nano /etc/netplan/01-router-config.yaml
```
**Contenido del archivo:**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
      optional: true
    enp2s0:
      addresses: [192.168.130.1/24]
    enp3s0:
      addresses: [192.168.30.1/24]
```
```bash
# Aplicar configuración de red
sudo netplan apply
```
**Explicación:** Configura las tres interfaces de red del router con sus IPs correspondientes para DMZ, INTRANET y NAT.

---

## 2. CONFIGURACIÓN DEL HOSTNAME

```bash
# Establecer nombre del router
sudo hostnamectl set-hostname R-N03
```
**Explicación:** Cambia el nombre del equipo a R-N03 para identificarlo fácilmente en la red.

---

## 3. HABILITAR IP FORWARDING

```bash
# Editar configuración del kernel
sudo nano /etc/sysctl.conf
```
**Añadir estas líneas:**
```
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```
```bash
# Aplicar cambios inmediatamente
sudo sysctl -p
```
**Explicación:** Permite que el router reenvíe paquetes entre redes, habilitando el routing.

---

## 4. INSTALAR Y CONFIGURAR IPTABLES

```bash
# Instalar iptables y hacer reglas persistentes
sudo apt update
sudo apt install iptables iptables-persistent -y

# Configurar NAT para internet
sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE

# Permitir tráfico entre redes
sudo iptables -A FORWARD -i enp2s0 -o enp1s0 -j ACCEPT
sudo iptables -A FORWARD -i enp3s0 -o enp1s0 -j ACCEPT
sudo iptables -A FORWARD -i enp1s0 -o enp2s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp1s0 -o enp3s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -j ACCEPT
sudo iptables -A FORWARD -i enp3s0 -o enp2s0 -j ACCEPT

# Hacer reglas permanentes
sudo netfilter-persistent save
sudo systemctl enable netfilter-persistent
```
**Explicación:** Configura NAT y reglas de firewall para permitir el routing y la comunicación entre redes.

---

## 5. CREAR USUARIO BCHECKER

```bash
# Crear usuario bchecker
sudo useradd -m -s /bin/bash bchecker

# Establecer contraseña
echo "bchecker:Pw.bchecker121" | sudo chpasswd

# Dar permisos de administrador
sudo usermod -aG sudo bchecker
```
**Explicación:** Crea el usuario requerido por el proyecto y le da permisos de administrador.

---

## 6. INSTALAR HERRAMIENTAS DE RED

```bash
# Instalar herramientas de diagnóstico y SSH
sudo apt install iputils-ping net-tools dnsutils openssh-server -y

# Habilitar SSH
sudo systemctl enable ssh
sudo systemctl start ssh
```
**Explicación:** Instala utilidades necesarias para la administración y habilita el acceso remoto por SSH.

---

## 7. CONFIGURAR DNSMASQ (DNS + DHCP)

```bash
# Hacer backup de archivos originales
sudo mkdir /etc/backup_dns_config
sudo cp /etc/dnsmasq.conf /etc/backup_dns_config/dnsmasq.conf.original
sudo cp /etc/resolv.conf /etc/backup_dns_config/resolv.conf.original

# Detener systemd-resolved
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved

# Crear nuevo resolv.conf
sudo rm -f /etc/resolv.conf
sudo nano /etc/resolv.conf
```
**Contenido de resolv.conf:**
```
nameserver 127.0.0.1
nameserver 8.8.8.8
nameserver 8.8.4.4
search local
```
```bash
# Configurar dnsmasq
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.original
sudo nano /etc/dnsmasq.conf
```
**Contenido de dnsmasq.conf:**
```
interface=enp2s0
interface=enp3s0
except-interface=lo
except-interface=enp1s0

bind-interfaces
listen-address=127.0.0.1
listen-address=192.168.130.1
listen-address=192.168.30.1

dhcp-range=enp2s0,192.168.130.10,192.168.130.100,255.255.255.0,24h
dhcp-option=enp2s0,3,192.168.130.1
dhcp-option=enp2s0,6,192.168.130.1

dhcp-range=enp3s0,192.168.30.10,192.168.30.100,255.255.255.0,24h
dhcp-option=enp3s0,3,192.168.30.1
dhcp-option=enp3s0,6,192.168.30.1

address=/R-N03/192.168.130.1
address=/R/192.168.130.1
address=/W-N03/192.168.130.2
address=/B-N03/192.168.130.3
address=/F-N03/192.168.130.4
address=/PC-1.03/192.168.30.2
address=/PC-2.03/192.168.30.3

server=8.8.8.8
server=8.8.4.4

cache-size=1000
log-queries
log-dhcp
dhcp-authoritative
```
```bash
# Iniciar dnsmasq
sudo systemctl enable dnsmasq
sudo systemctl start dnsmasq
```
**Explicación:** Configura el servicio combinado de DNS y DHCP para las redes DMZ e INTRANET.

---

## 8. VERIFICAR CONFIGURACIÓN DE RED

```bash
# Ver interfaces de red
ip a

# Ver tabla de rutas
ip route show

# Ver IP forwarding
cat /proc/sys/net/ipv4/ip_forward

# Probar conectividad
ping -c 3 192.168.130.1
ping -c 3 192.168.30.1
ping -c 3 8.8.8.8
```
**Explicación:** Verifica que la red esté configurada correctamente y que haya conectividad entre interfaces y hacia internet.

---

## 9. VERIFICAR NAT Y FIREWALL

```bash
# Ver reglas NAT
sudo iptables -t nat -L -v

# Ver reglas de forwarding
sudo iptables -L FORWARD -v

# Verificar persistencia
sudo netfilter-persistent status
```
**Explicación:** Comprueba que las reglas de NAT y firewall estén activas y funcionando.

---

## 10. VERIFICAR DNS

```bash
# Ver estado del servicio
sudo systemctl status dnsmasq

# Probar resolución DNS
nslookup R-N03
nslookup R
nslookup W-N03
nslookup google.com

# Ver logs de DNS
sudo journalctl -u dnsmasq -n 10
```
**Explicación:** Verifica que el servicio DNS funcione correctamente y resuelva nombres internos y externos.

---

## 11. VERIFICAR DHCP

```bash
# Ver configuración DHCP
sudo cat /etc/dnsmasq.conf | grep dhcp-range

# Ver leases DHCP
sudo cat /var/lib/misc/dnsmasq.leases

# Ver puertos de escucha
sudo netstat -tulpn | grep :53
sudo netstat -tulpn | grep :67

# Monitorizar DHCP en tiempo real
sudo journalctl -u dnsmasq -f
```
**Explicación:** Comprueba que el servicio DHCP esté activo y asignando direcciones correctamente.

---

## 12. VERIFICACIÓN COMPLETA DEL SISTEMA

```bash
# Script de verificación completa
echo "=== VERIFICACIÓN COMPLETA ROUTER R-N03 ==="
echo "1. Hostname: $(hostname)"
echo "2. Interfaces:"
ip a | grep -E "(enp1s0|enp2s0|enp3s0)" | grep inet
echo "3. IP Forwarding: $(cat /proc/sys/net/ipv4/ip_forward)"
echo "4. DNSmasq Status: $(systemctl is-active dnsmasq)"
echo "5. DHCP Leases:"
sudo cat /var/lib/misc/dnsmasq.leases 2>/dev/null || echo "No hay leases"
echo "6. Usuario bchecker: $(id bchecker 2>/dev/null && echo "EXISTE" || echo "NO EXISTE")"
```
**Explicación:** Proporciona un resumen completo del estado y configuración del router.

---

## 13. RESPALDAR CONFIGURACIÓN

```bash
# Crear backup completo
sudo tar -czf /root/router-backup-$(date +%Y%m%d).tar.gz /etc/netplan/ /etc/dnsmasq.conf /etc/resolv.conf /etc/iptables/

# Listar backups
sudo ls -la /root/*.tar.gz
```
**Explicación:** Permite crear y listar copias de seguridad de la configuración del router.

---

## 14. RESTAURAR CONFIGURACIÓN ORIGINAL

```bash
# Restaurar DNS original
sudo systemctl stop dnsmasq
sudo systemctl disable dnsmasq
sudo cp /etc/backup_dns_config/dnsmasq.conf.original /etc/dnsmasq.conf
sudo cp /etc/backup_dns_config/resolv.conf.original /etc/resolv.conf
sudo systemctl start systemd-resolved
sudo systemctl enable systemd-resolved
```

## 14.Rutas

```bash
 ip route show

default via 192.168.120.1 dev enp1s0 proto dhcp src 192.168.123.142 metric 100 
192.168.30.0/24 dev enp3s0 proto kernel scope link src 192.168.30.1 
192.168.120.0/22 dev enp1s0 proto kernel scope link src 192.168.123.142 metric 100 
192.168.120.1 dev enp1s0 proto dhcp scope link src 192.168.123.142 metric 100 
192.168.130.0/24 dev enp2s0 proto kernel scope link src 192.168.130.1 

```

---