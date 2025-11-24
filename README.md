# P0.0 - Despliegue de la Infraestructura

## Estructura del proyecto

### 📁 Configuración de servicios (`configs`)
- [Configuración del servidor de base de datos](configs/bbddserv.md)
- [Configuración del cliente Linux](configs/cliente_linux.md)
- [Configuración del cliente Windows](configs/cliente_Win.md)
- [Configuración del servidor FTP](configs/ftp.md)
- [Configuración del router](configs/ruter.md)
- [Configuración del servidor web](configs/webserver.md)

### 📁 Documentación técnica (`docs`)
- [Creación del repositorio Git](docs/creacion_Rep_Git.md)
- [Diagrama de la infraestructura](docs/diagrama.md)
- [Investigación de tecnologías](docs/investigacion.md)
- [Requisitos del proyecto](docs/requisitos.md)
- [Tabla de configuración de IPs](docs/tabla_ip.md)




## Información del Proyecto

- **Módulo:** 0379 - Proyecto intermodular en administración de sistemas informáticos en red
- **Actividad:** P0.0 - Despliegue de infraestructura
- **Grupo:** ASIXc2AC-G03
- **Duración:** 6 semanas (finaliza 1/12/2025)
- **Metodología:** Sprints quincenales (3 sprints de 10 horas cada uno)

## Objetivo

El objetivo principal consiste en desplegar una infraestructura completa y funcional para una aplicación multicapa que integre los siguientes servicios y componentes:

- Servidor web (Apache2)
- Monitorización de red
- Acceso remoto por SSH
- Base de datos (MySQL)
- Servidor DHCP
- Servidor DNS
- Servidor FTP

---

## Especificaciones Técnicas

### Hardware y Redes

- **Router principal:** R-N03, equipado con interfaces para tres redes separadas:
  - DMZ
  - INTRANET
  - NAT

### Servidores Desplegados

- **Web Server:** W-N03 (Apache2)
- **Servidor SSH**
- **Base de datos:** B-N03 (MySQL)
- **Servidor DHCP**
- **Servidor DNS**
- **Servidor FTP:** F-N03 (vsftpd)

### Clientes

- **PC-1.03:** Estación cliente Windows
- **PC-2.03:** Estación cliente Linux

---

## Credenciales Comunes

- **Usuario global:** bchecker
- **Contraseña:** bchecker121
- Configurado en todos los sistemas y servicios para administración y pruebas comunes

---

## Dataset Utilizado

- **Conjunto de datos:** Equipamientos educativos de la ciudad de Barcelona (OpenData Barcelona)
- **Formato:** CSV
- **Contenido:** Listado y detalles de los equipamientos educativos públicos

---

## Inventario de Equipos y Configuración de Red

| Nombre del Equipo | Servicio          | IPs                        | Red Virtual           | Función                                   |
|-------------------|-------------------|----------------------------|----------------------|-------------------------------------------|
| W-N03             | Web Server        | 192.168.130.2              | DMZ                  | Servidor web (Apache2)                    |
| R-N03             | Router            | 192.168.130.1, 192.168.30.1| NAT, DMZ, INTRANET    | Enrutamiento y enlace entre todas las redes|
| B-N03             | Base de Datos     | 192.168.30.4               | INTRANET             | Servidor de base de datos (MySQL)         |
| F-N03             | FTP Server        | 192.168.130.4              | DMZ                  | Servidor FTP (vsftpd)                     |
| PC-1.03           | Cliente Windows   | 192.168.30.2               | INTRANET             | Estación de trabajo cliente               |
| PC-2.03           | Cliente Linux     | 192.168.30.3               | INTRANET             | Estación de trabajo cliente               |

---

## Resumen de Componentes y Flujos

- **Web Server y FTP Server:** Ubicados en la DMZ para ofrecer servicios externos con un nivel elevado de seguridad.
- **Base de Datos y clientes:** Ubicados exclusivamente en la INTRANET, protegidos de acceso externo.
- **Router:** Conecta y gestiona el tráfico entre redes (NAT, DMZ, INTRANET) y permite salida controlada a Internet.
- **DNS y DHCP:** Proveen servicios esenciales de resolución y asignación de IPs para los equipos y servidores.
- **SSH:** Acceso remoto administrado a todos los servidores para tareas operativas y control.

Toda la configuración y documentación técnica está centralizada en el repositorio del proyecto para un despliegue reproducible y una administración eficiente.

