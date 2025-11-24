# Tabla de Configuración de IPs - Proyecto P0.0-ASIXc2AC-G03

## Especificación de Equipos y Redes

| Servicio      | Nombre del Equipo | IPs                        | Redes Virtuales         | Función                                      |
|---------------|-------------------|----------------------------|------------------------|----------------------------------------------|
| Web Server    | W-N03             | 192.168.130.2              | DMZ                    | Servidor web (Apache2)                       |
| Router        | R-N03             | 192.168.130.1, 192.168.30.1| NAT, DMZ, INTRANET      | Puerta de enlace, interconexión de redes     |
| Base de Datos | B-N03             | 192.168.30.4               | INTRANET               | Servidor de base de datos (MySQL)            |
| FTP Server    | F-N03             | 192.168.130.4              | DMZ                    | Servidor FTP (vsftpd)                        |
| Cliente Win   | PC-1.03           | 192.168.30.92              | INTRANET               | Estación cliente Windows                     |
| Cliente Linux | PC-2.03           | 192.168.30.43              | INTRANET               | Estación cliente Linux                       |

---

## Resumen de Configuración de Redes

### Red DMZ (Zona Desmilitarizada)

- Subred: 192.168.130.0/24
- Equipos principales:
  - W-N03 (Web Server - Apache2): 192.168.130.2
  - F-N03 (FTP Server - vsftpd): 192.168.130.4
  - R-N03 (Router): 192.168.130.1 (interfaz DMZ)

### Red INTRANET

- Subred: 192.168.30.0/24
- Equipos principales:
  - PC-1.03 (Cliente Windows): 192.168.30.92
  - PC-2.03 (Cliente Linux): 192.168.30.43
  - B-N03 (Base de Datos - MySQL): 192.168.30.4
  - R-N03 (Router): 192.168.30.1 (interfaz INTRANET)

### Red NAT

- Gateway principal: R-N03 (Router) como salida a Internet
- Función: Permite acceso externo controlado para los equipos internos

---

## Notas de Configuración

- El usuario estándar en todos los sistemas es `bchecker` con la contraseña `bchecker121`
- El router R-N03 cuenta con múltiples interfaces para enlazar NAT, DMZ e INTRANET de manera segura
- Los servidores web (Apache2) y FTP (vsftpd) operan en la DMZ, aumentando la seguridad perimetral al límite entre interno y externo
- La base de datos (MySQL) y los clientes sólo son accesibles desde la INTRANET, asegurando que el acceso crítico queda restringido
- Todas las configuraciones están documentadas y controladas por versiones en el repositorio del proyecto
