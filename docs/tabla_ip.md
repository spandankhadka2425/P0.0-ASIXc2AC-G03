# Tabla de Configuración de IPs - Proyecto P0.0-ASIXc2AC-G03

## 📊 Especificación de Equipos y Redes

| Servicio | Nombre del Equipo | IPs | Redes Virtuales | Función |
|----------|-------------------|-----|-----------------|---------|
| Web Server | W-N03 | 192.168.130.2 | DMZ | Servidor Web |
| Router | R-N03 | 192.168.130.1<br>192.168.30.1 | NAT<br>DMZ<br>INTRANET | Salida a internet<br>Red para servers<br>Red interna |
| BBDD | B-N03 | 192.168.30.4 | INTRANET | Servidor MySQL |
| FTP | F-N03 | 192.168.130.4 | DMZ | Servidor FTP |
| C. Windows | PC-1.03 | 192.168.30.92 | INTRANET | Cliente |
| C. Linux | PC-2.03 | 192.168.30.12 | INTRANET | Cliente |

## 🌐 Resumen de Configuración de Redes

### Red DMZ (Zona Desmilitarizada)

* **Subred:** 192.168.130.0/24
* **Equipos:**
  * Web Server (W-N03): 192.168.130.2
  * FTP Server (F-N03): 192.168.130.4
  * Router (R-N03): 192.168.130.1

### Red INTRANET

* **Subred:** 192.168.30.0/24
* **Equipos:**
  * Cliente Windows (PC-1.03): 192.168.30.2
  * Cliente Linux (PC-2.03): 192.168.30.3
  * Base de Datos (B-N03): 192.168.30.4
  * Router (R-N03): 192.168.30.1

### Red NAT

* **Gateway:** Router (R-N03)
* **Función:** Salida a internet

## 🔧 Notas de Configuración

* Usuario estándar: `bchecker` con contraseña `bchecker121` en todos los equipos
* Router R-N03 tiene múltiples interfaces para conectar las tres redes
* Servidores Web y FTP ubicados en DMZ para mayor seguridad
* Base de Datos y clientes ubicados en INTRANET para acceso interno seguro