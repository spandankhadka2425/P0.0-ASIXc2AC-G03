# Tabla de Configuración de IPs - Proyecto P0.0-ASIXc2AC-G03

## 📊 Especificación de Equipos y Redes

| Servicio | Nombre del Equipo | IP | Red Virtual | Función |
|----------|-------------------|-----|-------------|----------|
| **WebServer** | W-N03 | 192.168.130.2 | DMZ | Servidor Web |
| **Router** | R-N03 | 192.168.130.1 | DMZ | Red para servidores |
| **Router** | R-N03 | 192.168.30.1 | INTRANET | Red para usuarios |
| **BBDD** | B-N03 | 192.168.130.3 | DMZ | Servidor MySQL |
| **FTP** | F-N03 | 192.168.130.4 | DMZ | Servidor FTP |
| **Cliente Windows** | PC-1.03 | 192.168.30.2 | INTRANET | Cliente |
| **Cliente Linux** | PC-2.03 | 192.168.30.3 | INTRANET | Cliente |

## 🌐 Resumen de Configuración de Redes

### **Red DMZ (Zona Desmilitarizada)**
- **Subred:** 192.168.130.0/24
- **Equipos:**
  - Web Server (W-N03): 192.168.130.2
  - Base de Datos (B-N03): 192.168.130.3
  - FTP Server (F-N03): 192.168.130.4
  - Router (R-N03): 192.168.130.1

### **Red INTRANET**
- **Subred:** 192.168.30.0/24
- **Equipos:**
  - Cliente Windows (PC-1.03): 192.168.30.2
  - Cliente Linux (PC-2.03): 192.168.30.3
  - Router (R-N03): 192.168.30.1

### **Red NAT**
- **Gateway:** Router (R-N03): 192.168.130.1
- **Función:** Salida a internet

## 🔧 Notas de Configuración

- **Usuario estándar:** `bchecker` con contraseña `bchecker121` en todos los equipos
- **Router R-N03** tiene múltiples interfaces para conectar las tres redes
- **Servidores** ubicados en DMZ para mayor seguridad
- **Clientes** ubicados en INTRANET para acceso interno
