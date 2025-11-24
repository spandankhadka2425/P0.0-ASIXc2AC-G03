```markdown
# Planificación del Proyecto - P0.0-ASIXc2AC-G03

## Requisitos del Proyecto

### Gestión de Tareas

* **Plataforma utilizada:** Proofhub
* **Planificación:** Todas las tareas se gestionan en Proofhub
* **Seguimiento:** Control del progreso con tableros Kanban

### Metodología Scrum

* **Sprints:** Tres sprints, cada uno de dos semanas
* **Duración por sprint:** 10 horas (5 horas por semana)
* **Duración total del proyecto:** 6 semanas

### Estructura del Proyecto

* **Nombre del Proyecto:** P0.0-ASIXc2AC-G03
* **Nomenclatura general:** P0.0-ASIXc2gC-Gnn  
  * `g` = grupo (A o B)  
  * `nn` = número de grupo con dos dígitos

### Definición de Tareas

* **Backlog General:** Todas las tareas en el backlog de Proofhub
* **Backlog por Sprint:** Prioridad y asignación por sprint
* **Arquitectura:** Diagramas definidos y documentados

---

## Configuración de Seguridad

### Usuario de Acceso

* **Usuario global:** bchecker
* **Contraseña:** bchecker121
* **Alcance:** Sistemas, servidores y aplicaciones
* **Objetivo:** Acceso uniforme y controlado para el equipo

---

## Nomenclatura de Equipos

### Formato de nombres

* **Formato:** X-NCC  
  * `X` = Tipo de servidor (W=Web, R=Router, B=BBDD, F=FTP)  
  * `N` = Identificador de red  
  * `CC` = Número de equipo (03)

* **Ejemplos:**
  * W-N03 (Web Server)
  * R-N03 (Router)
  * B-N03 (Base de Datos)
  * F-N03 (FTP Server)
  * PC-1.03 (Cliente Windows)
  * PC-2.03 (Cliente Linux)

---

## Control de Versiones y Documentación

### Repositorio Git

* **Nombre:** P0.0-ASIXc2AC-G03
* **Contenido:**
  * Configuración completa de servicios y servidores
  * Documentación técnica detallada
  * Diagramas de arquitectura
  * Scripts de instalación o automatización
  * Archivos de configuración (.conf, .yaml, etc.)
  * Manual de instalación y despliegue

---

## Cronograma del Proyecto

### Fechas de los Sprints

| Sprint    | Fecha de inicio | Fecha de fin  | Duración | Estado      |
|-----------|----------------|--------------|----------|-------------|
| Sprint 1  | 20/10/2025     | 3/11/2025    | 2 semanas, 10h | Finalizado  |
| Sprint 2  | 4/11/2025      | 17/11/2025   | 2 semanas, 10h | Finalizado  |
| Sprint 3  | 18/11/2025     | 1/12/2025    | 2 semanas, 10h | En progreso |

*Total horas del proyecto: 30 horas*

---

## Entregables por Sprint

### Sprint 1 - Planificación e Infraestructura Base

**Tareas realizadas:**
- Planificación inicial del proyecto en Proofhub
- Diseño del diagrama de arquitectura de red
- Configuración de credenciales estándar
- Documentación y topología
- Despliegue de clientes (Windows y Linux)

**Entregables:**
- Diagrama de arquitectura documentado
- Repositorio Git configurado
- Planificación en Proofhub
- Clientes desplegados y operativos

---

### Sprint 2 - Servicios de Red y Servidores

**Tareas realizadas:**
- Despliegue del servidor de BBDD (B-N03) con MySQL
- Despliegue del servidor Web (W-N03) con Apache2
- Despliegue del servidor FTP (F-N03) con vsftpd
- Configuración de servicios adicionales según requisitos del Sprint

**Entregables:**
- Servidor MySQL operativo en INTRANET
- Servidor Web funcional en DMZ
- Servidor FTP configurado en DMZ
- Documentación detallada de configuraciones

---

### Sprint 3 - Servicios Avanzados y Finalización

**Tareas planificadas:**
- Despliegue del router R-N03
- Corrección y optimización de la base de datos
- Preparar las tareas finales del Sprint 3
- Completar documentación en GitHub
- Configuración del servidor DHCP
- Configuración del servidor DNS
- Pruebas integradas de infraestructura

**Entregables:**
- Router configurado para conectar todas las redes (NAT, DMZ, INTRANET)
- Servidor DHCP operativo
- Servidor DNS configurado con Bind9
- Base de datos revisada y optimizada
- Documentación final y pruebas de funcionamiento
- Informe final del proyecto

---

## Diagrama de Arquitectura

### Elementos del Diagrama

**Componentes de red:**
- DMZ (192.168.130.0/24)
  - Web Server (W-N03) - Apache2: 192.168.130.2
  - FTP Server (F-N03) - vsftpd: 192.168.130.4
  - Router (R-N03) - gateway: 192.168.130.1

- INTRANET (192.168.30.0/24)
  - Cliente Windows (PC-1.03): 192.168.30.2
  - Cliente Linux (PC-2.03): 192.168.30.3
  - Base de Datos (B-N03) - MySQL: 192.168.30.4
  - Router (R-N03): 192.168.30.1

- NAT / Internet: Salida por router R-N03

**Servicios desplegados:**
- Web Server: Apache2
- Base de Datos: MySQL
- FTP Server: vsftpd
- DNS Server: Bind9
- DHCP Server: isc-dhcp-server

**Clientes:**
- PC-1.03: Windows
- PC-2.03: Linux

**Flujo de conectividad:**
- Acceso web desde Internet a DMZ y servidor web
- Acceso a BBDD desde el servidor web (DMZ → INTRANET)
- Transferencia de archivos por FTP (DMZ)
- Resolución de nombres por DNS y asignación IP por DHCP
- Todos los equipos tienen salida a Internet a través del router

---

## Objetivos de Calidad

- Documentación completa y actualizada en el repositorio Git
- Configuración segura de credenciales y segmentación de redes
- Funcionamiento y pruebas de todos los servicios desplegados
- Conectividad e integración entre todos los componentes de la arquitectura
- Seguimiento y actualización constante de tareas en Proofhub
```