# 💻 Análisis Técnico Extendido de las Tecnologías del Proyecto

## Introducción

Este documento investiga en profundidad las tecnologías seleccionadas para el proyecto de infraestructura multicapa **P0.0-ASIXc2AC-G03**. Cada apartado incluye una comparativa detallada frente a las alternativas más habituales, fundamentando la decisión para este entorno académico con plazos ajustados y trabajo en equipo. El objetivo es mostrar que cada elección responde a criterios técnicos y educativos específicos, no a preferencias arbitrarias.

---

## 🐧 Ubuntu Server vs Debian - Sistema Operativo Base

**Desarrollador:** Canonical Ltd.  
**Empresas relevantes:** Netflix, Wikipedia, Snapchat, eBay.

### Características Técnicas Fundamentales

Ubuntu Server y Debian comparten la misma raíz: ambos derivan del kernel Linux y emplean APT como gestor de paquetes, con soporte para arquitecturas modernas y optimizaciones multiusuario. Ambos reciben actualizaciones de seguridad rápidas y disponen de un ecosistema de herramientas listo para servidores web, bases de datos y virtualización.

#### Ubuntu Server:
- Ciclo de soporte claro: 5 años de soporte LTS.
- Incluye por defecto **Systemd**, *AppArmor*, **UFW** y herramientas para virtualización y entornos cloud, como **Netplan** y *cloud-init*.
- Dispone de imágenes pre-optimizadas para escenarios de virtualización (IsardVDI, Proxmox).

#### Debian:
- Famoso por su estabilidad y minimalismo.
- Dispone de versiones muy estables, aunque los paquetes suelen estar menos actualizados.
- Instalaciones más personalizables y "limpias" (menos paquetes preinstalados).

### Comparativa y Elección

| Característica          | Ubuntu Server (Elegido) | Debian           |
|------------------------ |:----------------------:|:----------------:|
| Instalación guiada y rápida | Sí                   | Menos amigable   |
| Documentación educativa | Muy abundante           | Más técnica      |
| Versionado de paquetes  | Recientes (LTS)         | Conservador      |
| Integración con entornos cloud | Destacada         | Menor foco       |
| Configuración de red moderna | Netplan por defecto | Interfaces tradicional |

#### Razonamiento y Decisión

En este proyecto se han estudiado ambas alternativas. Se elige **Ubuntu Server LTS** porque en un entorno educativo donde cada hora cuenta, simplifica tanto la instalación como la configuración inicial: perfiles predefinidos, mejores mensajes y soporte para usuarios nuevos, y documentación paso a paso orientada a la docencia. Además, la implementación por defecto de herramientas como Netplan facilita la configuración de redes en entornos virtualizados, clave para avanzar rápidamente en los sprints. Debian sigue siendo una opción sólida para entornos de producción que priorizan estabilidad extrema, pero para esta experiencia de aprendizaje Ubuntu permite mayor velocidad y facilidad de uso[web:3][web:4][web:8].

---

## 🗄️ MySQL vs PostgreSQL - Sistema de Gestión de Bases de Datos Relacional

**Desarrollador:** Oracle Corporation  
**Empresas relevantes:** Facebook, YouTube, Twitter, Booking.

### Características Técnicas Fundamentales

**MySQL**:
- Motor por defecto: InnoDB (transacciones ACID, bloqueo a nivel de fila, alta concurrencia).
- Soporta importación directa de datos con `LOAD DATA INFILE`.
- Excelente integración con PHP y herramientas como phpMyAdmin.

**PostgreSQL**:
- Motor avanzado, destaca en tipos de datos personalizados, consultas analíticas, extensiones.
- Sintaxis SQL estricta, orientación a aplicaciones avanzadas y de requisitos especiales.

### Comparativa y Elección

| Característica                      | MySQL (Elegido)     | PostgreSQL     |
| ----------------------------------- |:-------------------:|:--------------:|
| Integración con PHP                 | Nativa, directa     | Requiere módulos extra |
| Importación CSV (Sprint 2)          | `LOAD DATA INFILE` rápido | `COPY` con permisos y edición |
| Herramientas de administración gráfica | phpMyAdmin simple | PgAdmin, más avanzada pero menos conocida por el alumnado|
| Config. de acceso remoto            | Por SQL únicamente  | Editar varios archivos de configuración |
| Documentación / comunidad educativa | Muy abundante       | Técnica, menos enfocada a principiantes |

#### Razonamiento y Decisión

Aunque PostgreSQL es considerado el RDBMS abierto más avanzado, para este caso lo prioritario es reducir la curva de entrada: MySQL resuelve la importación de datos (Sprint 2) de forma directa, la integración con PHP es plug-and-play en Ubuntu, y la administración vía phpMyAdmin facilita la autoevaluación y validación de prácticas. Además, los comandos complejos de PostgreSQL no son necesarios aquí y podrían añadir complejidad. Por todo ello, MySQL 8.0 resulta el equilibrio óptimo para lograr los objetivos del módulo sin dispersar esfuerzos en configuración avanzada[web:6][web:12][web:15][web:19].

---

## 🌐 Netplan vs Configuración Tradicional Debian
**Desarrollador:** Canonical Ltd.

### Características Técnicas Fundamentales

**Netplan** usa archivos YAML para definir toda la configuración de red de una máquina, utilizando *backends* modernos como systemd-networkd. Incluye el comando `netplan try`, que permite aplicar cambios temporalmente y revierte si se pierde conectividad, muy útil en entornos virtualizados donde no se tiene acceso físico.

**Interfaz tradicional de Debian:** 
- Configuración mediante `/etc/network/interfaces` con sintaxis clásica y archivos dispersos.
- Menos amigable con el versionado y auditoría de cambios.

### Comparativa y Elección

| Característica                  | Netplan (Elegido)      | Interfaces tradicional |
| ------------------------------- |:----------------------:|:---------------------:|
| Legibilidad y estructura        | YAML legible           | Sintaxis tradicional  |
| Seguridad ante errores          | `netplan try` revierte | No existe rollback automático |
| Versionado en Git               | Sencillo (archivos únicos) | Fragmentado          |

#### Razonamiento y Decisión

La configuración de interfaces de red será una tarea fundamental en el Sprint 1 y, gracias a Netplan, se puede automatizar/documentar de manera fiable y segura. La capacidad de revertir cambios evita bloqueos en entornos virtuales (IsardVDI). El uso de archivos YAML, fácilmente gestionables en Git, fomenta la documentación y la trazabilidad. Para proyectos donde el aprendizaje y la reproducibilidad pesan más que la máxima personalización, Netplan es claramente ventajoso.

---

## 🐘 Apache HTTP Server vs Nginx

**Desarrollador:** Apache Software Foundation  
**Empresas relevantes:** Adobe, LinkedIn, Cisco, muchos hostings.

### Características y Comparativa

**Apache**:
- Modularidad, soporte nativo de PHP con mod_php/libapache2-mod-php.
- Permite `.htaccess` para reescritura y restricciones por directorio.
- Gran cantidad de documentación, comunidad y casos de uso educativo.

**Nginx**:
- Mayor rendimiento en contenido estático, necesita PHP-FPM para código PHP.
- No soporta `.htaccess`, requiere editar config global.
- Más habitual en escenarios de alta concurrencia.

| Característica               | Apache (Elegido)      | Nginx               |
|----------------------------- |:---------------------:|:-------------------:|
| Integración PHP              | Directa, rápida       | Requiere PHP-FPM y configuración extra |
| Configuración por proyecto   | `.htaccess` disponible | No disponible       |
| Curva de aprendizaje         | Documentación muy accesible | Más técnica      |
| Diagnóstico de errores       | Centralizado en Apache | Disperso en varios logs|

#### Razonamiento y Decisión

La prioridad en este caso es la facilidad y rapidez de integración con PHP (para el Sprint 3), la posibilidad de hacer cambios rápidos por proyecto/directorio sin reiniciar servicios y la abundancia de recursos didácticos en castellano para el *stack* LAMP clásico. Nginx es excelente en producción de alta demanda, pero añade una complejidad innecesaria aquí y no aporta ventajas para la baja carga estimada en este proyecto.

---

## ✨ Sinergias Integradas y Flujo de Aprendizaje

Las tecnologías elegidas forman un ecosistema homogéneo y actual:

- **Instalación y gestión:** Se instalan vía APT y se gestionan con Systemd, con logs centralizados y comandos homogéneos para todos los servicios.
- **Versionado:** Todos los archivos relevantes (Netplan, scripts SQL, Apache, PHP) bajo Git, asegurando trazabilidad y colaboración efectiva.
- **Curva de aprendizaje:** Crece incrementalmente: Surgen retos técnicos por sprint (redes, datos, web), pero se mantienen las herramientas base a lo largo de toda la materia.

**Resumen del cronograma:**

| Sprint    | Enfoque Principal     | Tecnologías clave                       |
|-----------|----------------------|------------------------------------------|
| Sprint 1  | Redes                | Netplan, Git, GNU/Linux, SSH            |
| Sprint 2  | Servicios             | MySQL, DHCP, DNS, Systemd               |
| Sprint 3  | Aplicación Web        | Apache, PHP, Conexión a MySQL, HTML/CSS |

---

## ✅ Conclusión

La elección de Ubuntu Server, MySQL, Netplan y Apache responde no solo a criterios técnicos, sino pedagógicos: ofrecen facilidad de uso, aprendizaje incremental, documentación y una experiencia lo más cercana posible a un entorno profesional estándar. Cada alternativa (Debian, PostgreSQL, Nginx) ha sido considerada y podría ser válida en otros contextos, pero para este proyecto concreto (6 semanas, entorno virtualizado, alumnado en formación), se ha priorizado la velocidad de puesta en marcha, la reducción de barreras de entrada y la coherencia en todo el ciclo de vida del proyecto.

El resultado es una base robusta, fácilmente documentable y extensible, que maximiza tanto el aprendizaje como la eficiencia durante los sprints.
