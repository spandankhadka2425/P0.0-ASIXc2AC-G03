# Planificación del Proyecto - P0.0-ASIXc2AC-G03

## 📋 Requisitos del Proyecto

### Gestión de Tareas

* **Plataforma:** Proofhub
* **Planificación:** Todas las tareas deben planificarse y gestionarse en Proofhub
* **Seguimiento:** Control de avance mediante tableros Kanban

### Metodología Scrum

* **Sprints:** Quincenales (cada 2 semanas)
* **Duración por Sprint:** 10 horas de trabajo efectivo
* **Distribución semanal:** 5 horas por semana
* **Total de Sprints:** 3 sprints
* **Duración total del proyecto:** 6 semanas

### Estructura del Proyecto

* **Nomenclatura del Proyecto:** `P0.0-ASIXc2gC-Gnn`
  * `g` = grupo (A o B)
  * `nn` = número de grupo con dos dígitos
* **Proyecto Específico:** `P0.0-ASIXc2AC-G03`

### Definición de Tareas

* **Backlog General:** Todas las tareas del proyecto listadas en el backlog de Proofhub
* **Backlog por Sprint:** Tareas asignadas a cada sprint según prioridad
* **Arquitectura:** Definir y documentar diagramas de la arquitectura a desplegar

## 🔐 Configuración de Seguridad Estándar

### Usuario de Acceso

* **Usuario:** `bchecker`
* **Contraseña:** `bchecker121`
* **Alcance:** Todos los sistemas, servidores y aplicaciones
* **Propósito:** Permitir el acceso uniforme a todos los equipos del proyecto

## 🖥️ Nomenclatura de Equipos

### Estructura de Nombres

* **Formato:** `X-NCC`
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

## 📁 Control de Versiones y Documentación

### Repositorio Git

* **Nombre del Repositorio:** `P0.0-ASIXc2gC-Gnn`
  * `g` = grupo (A o B)
  * `nn` = número de grupo con dos dígitos
* **Repositorio Específico:** `P0.0-ASIXc2AC-G03`

### Contenido del Repositorio

* Toda la configuración de servicios y servidores
* Documentación técnica completa
* Diagramas de arquitectura de red
* Scripts de implementación y automatización
* Archivos de configuración (.conf, .yaml, etc.)
* Manual de instalación y despliegue

## 🗓️ Cronograma del Proyecto

### Duración Total

* **Semanas:** 6 semanas
* **Sprints:** 3 sprints de 2 semanas cada uno
* **Horas Totales:** 30 horas (10 horas por sprint)

### Calendario de Sprints

#### Sprint 1 (COMPLETADO)
* **Fecha inicio:** 18 de noviembre
* **Fecha fin:** 3 de noviembre
* **Duración:** 2 semanas (10 horas)
* **Estado:** ✅ Finalizado

#### Sprint 2 (COMPLETADO)
* **Fecha inicio:** 3 de noviembre
* **Fecha fin:** 18 de diciembre
* **Duración:** 2 semanas (10 horas)
* **Estado:** ✅ Finalizado

#### Sprint 3 (EN CURSO)
* **Fecha inicio:** 18 de diciembre
* **Fecha fin:** 1 de diciembre
* **Duración:** 2 semanas (10 horas)
* **Estado:** 🔄 En progreso

## 📦 Entregables por Sprint

### Sprint 1 - Planificación e Infraestructura Base ✅
**Tareas completadas:**
1. Planificación inicial del proyecto (Proofhub + GitHub)
2. Diseño del diagrama de arquitectura de red
3. Configuración de credenciales estándar
4. Documentación y topología
5. Despliegue de clientes (Windows y Linux)

**Entregables:**
* Diagrama de arquitectura documentado
* Repositorio Git configurado
* Tareas planificadas en Proofhub
* Clientes desplegados y operativos

### Sprint 2 - Servicios de Red y Servidores ✅
**Tareas completadas:**
1. Despliegue del servidor de BBDD (B-N03)
2. Despliegue del servidor Web (W-N03)
3. Despliegue del servidor FTP (F-N03)
4. Investigación y preparación de servicios adicionales

**Entregables:**
* Servidor MySQL operativo en INTRANET
* Servidor Web funcional en DMZ
* Servidor FTP configurado en DMZ
* Documentación de configuraciones

### Sprint 3 - Servicios Avanzados y Finalización 🔄
**Tareas planificadas:**
1. Despliegue del router R-N03
2. Arreglar BBDD (corrección y optimización)
3. Preparación del Sprint 3 (ordenar tareas y prioridad)
4. Mejorar GitHub (documentación completa)
5. Configuración del servidor DHCP
6. Configuración del DNS Server
7. Pruebas integradas de infraestructura

**Entregables:**
* Router configurado con todas las redes (NAT, DMZ, INTRANET)
* Servidor DHCP operativo
* Servidor DNS configurado
* Base de datos optimizada
* Documentación completa en GitHub
* Pruebas de conectividad y funcionamiento
* Informe final del proyecto

## 📊 Diagrama de Arquitectura

### Requisitos del Diagrama

Definir y documentar el diagrama de arquitectura que incluya:

#### Componentes de Red
* **Red DMZ (192.168.130.0/24)**
  * Web Server (W-N03): 192.168.130.2
  * FTP Server (F-N03): 192.168.130.4
  * Gateway Router: 192.168.130.1

* **Red INTRANET (192.168.30.0/24)**
  * Cliente Windows (PC-1.03): 192.168.30.2
  * Cliente Linux (PC-2.03): 192.168.30.3
  * Base de Datos (B-N03): 192.168.30.4
  * Gateway Router: 192.168.30.1

* **Red NAT**
  * Router (R-N03) como gateway de salida a Internet

#### Servidores y Servicios
* **Web Server (W-N03)** - Apache/Nginx
* **Base de Datos (B-N03)** - MySQL
* **FTP Server (F-N03)** - vsftpd/ProFTPD
* **DNS Server** - Bind9/dnsmasq
* **DHCP Server** - isc-dhcp-server

#### Clientes
* **PC-1.03** - Windows (cliente de pruebas)
* **PC-2.03** - Linux (cliente de pruebas)

#### Conectividad y Flujos de Red
* Tráfico web desde Internet → DMZ → Web Server
* Acceso a BBDD desde Web Server → INTRANET → MySQL
* Transferencia de archivos → DMZ → FTP Server
* Resolución de nombres → DNS Server
* Asignación automática de IPs → DHCP Server
* Salida a Internet → Router NAT

## 🎯 Objetivos de Calidad

* **Documentación:** Completa y actualizada en GitHub
* **Seguridad:** Credenciales configuradas, redes segregadas
* **Funcionalidad:** Todos los servicios operativos y probados
* **Integración:** Conectividad entre todos los componentes
* **Seguimiento:** Tareas actualizadas en Proofhub
