# P0.0 - Desplegament de la Infraestructura

## 📋 Información del Proyecto
- **Módulo:** 0379 - Projecte intermodular d'administració de sistemes informàtics en xarxa
- **Actividad:** P0.0 - Desplegament d'infraestructura
- **Grupo:** ASIXc2AC-G03
- **Duración:** 6 semanas (hasta 18/11)
- **Metodología:** Sprints quincenales (3 sprints de 10h cada uno)

## 🎯 Objetivo
Desplegar una infraestructura completa para una aplicación multicapa que incluya:
- Web Server
- Monitor de xarxes
Nombre del Equipo: W-N03

Servicio: Web Server

IP: 192.168.3.10

Red Virtual: DMZ

Función: Servidor Web


Nombre del Equipo: R-N03

Servicio: Router

IP: 192.168.3.11

Red Virtual: NAT, DMZ, INTRANET

Función: Salida a internet / Enrutador entre redes


Nombre del Equipo: B-N03

Servicio: Base de Datos

IP: 192.168.3.12

Red Virtual: DMZ

Función: Servidor MySQL


Nombre del Equipo: F-N03

Servicio: FTP

IP: 192.168.3.13

Red Virtual: DMZ

Función: Servidor FTP


Nombre del Equipo: PC-1.03

Servicio: Cliente Windows

IP: 192.168.3.14

Red Virtual: INTRANET

Función: Cliente


Nombre del Equipo: PC-2.03

Servicio: Cliente Linux

IP: 192.168.3.15

Red Virtual: INTRANET

Función: Cliente
- SSH
- Base de datos (MySQL)
- DHCP
- DNS
- FTP

## 🔧 Especificaciones Técnicas

### Hardware de Xarxa
- **Router:** R-NCC con 3 redes:
  - DMZ
  - Intranet
  - NAT

### Servidores a Desplegar
- **Web Server:** W-NCC
- **SSH Server**
- **Base de Datos:** B-NCC (MySQL)
- **DHCP Server**
- **DNS Server**
- **FTP Server:** F-NCC

### Clientes
- 1 PC Windows
- 1 PC Linux

## 🔐 Credenciales Comunes
- **Usuario:** `bchecker`
- **Contraseña:** `bchecker121`
- *Debe estar configurado en todos los sistemas y servicios*

## 📊 Dataset
**Equipamientos de educación - Ciudad de Barcelona**
- **Fuente:** [OpenData Ajuntament de Barcelona](https://opendata-ajuntament.barcelona.cat/data/dataset/f36b60f2-9541-4d08-b0f9-b0a9313fab3d)
- **Formato:** CSV
- **Contenido:** Listado de equipamientos educativos de la ciudad


