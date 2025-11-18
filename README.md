# P0.0 - Desplegament de la Infraestructura

## 📋 Información del Proyecto

* **Módulo:** 0379 - Projecte intermodular d'administració de sistemes informàtics en xarxa
* **Actividad:** P0.0 - Desplegament d'infraestructura
* **Grupo:** ASIXc2AC-G03
* **Duración:** 6 semanas (hasta 18/11)
* **Metodología:** Sprints quincenales (3 sprints de 10h cada uno)

## 🎯 Objetivo

Desplegar una infraestructura completa para una aplicación multicapa que incluya:

* Web Server
* Monitor de xarxes
* SSH
* Base de datos (MySQL)
* DHCP
* DNS
* FTP

## 🔧 Especificaciones Técnicas

### Hardware de Xarxa

* **Router:** R-N03 con 3 redes:
  * DMZ
  * INTRANET
  * NAT

### Servidores a Desplegar

* **Web Server:** W-N03
* **SSH Server**
* **Base de Datos:** B-N03 (MySQL)
* **DHCP Server**
* **DNS Server**
* **FTP Server:** F-N03

### Clientes

* 1 PC Windows (PC-1.03)
* 1 PC Linux (PC-2.03)

## 🔐 Credenciales Comunes

* **Usuario:** `bchecker`
* **Contraseña:** `bchecker121`
* Debe estar configurado en todos los sistemas y servicios

## 📊 Dataset

**Equipamientos de educación - Ciudad de Barcelona**

* **Fuente:** OpenData Ajuntament de Barcelona
* **Formato:** CSV
* **Contenido:** Listado de equipamientos educativos de la ciudad

## 📋 Inventario de Equipos

| Nombre del Equipo | Servicio | IP | Red Virtual | Función |
|-------------------|----------|----|--------------|---------| 
| W-N03 | Web Server | 192.168.130.2 | DMZ | Servidor Web |
| R-N03 | Router | 192.168.130.1<br>192.168.30.1 | NAT, DMZ, INTRANET | Salida a internet / Enrutador entre redes |
| B-N03 | Base de Datos | 192.168.30.4 | INTRANET | Servidor MySQL |
| F-N03 | FTP | 192.168.130.4 | DMZ | Servidor FTP |
| PC-1.03 | Cliente Windows | 192.168.30.2 | INTRANET | Cliente |
| PC-2.03 | Cliente Linux | 192.168.30.3 | INTRANET | Cliente |

