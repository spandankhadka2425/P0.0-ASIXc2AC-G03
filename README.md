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

## 🗓️ Planificación - Sprints

### Sprint 1 (Semanas 1-2 | 10h)
**Objetivo:** Planificación y configuración inicial
- [ ] Definir arquitectura de red y crear diagrama
- [ ] Configurar repositorio Git
- [ ] Instalar y configurar Router (R-NCC) con 3 redes
- [ ] Instalar y configurar Web Server (W-NCC)
- [ ] Configurar servicio SSH

### Sprint 2 (Semanas 3-4 | 10h)
**Objetivo:** Servicios de red y base de datos
- [ ] Instalar y configurar Base de Datos (B-NCC) con MySQL
- [ ] Descargar y cargar dataset de equipamientos educativos
- [ ] Configurar servicios DHCP y DNS
- [ ] Instalar y configurar FTP Server (F-NCC)

### Sprint 3 (Semanas 5-6 | 10h)
**Objetivo:** Clientes, aplicación web y pruebas
- [ ] Configurar PC Windows y Linux
- [ ] Desarrollar aplicación web para mostrar datos de la BBDD
- [ ] Realizar pruebas de conectividad y servicios
- [ ] Verificar acceso con usuario `bchecker` en todos los sistemas
- [ ] Documentación final

## 📁 Estructura del Repositorio