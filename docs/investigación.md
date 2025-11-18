# 💻 Análisis Técnico de las Tecnologías del Proyecto

## Introducción

Este documento presenta una investigación técnica sobre las tecnologías seleccionadas para el proyecto de infraestructura multicapa **P0.0-ASIXc2AC-G03**. Se analizan las características fundamentales de cada componente, se justifica su elección frente a alternativas del mercado y se explica por qué resultan óptimas para los requisitos específicos del proyecto que debe completarse en **6 semanas** distribuidas en **3 sprints**.

---

## 🐧 Ubuntu Server - Sistema Operativo Base

**Desarrollador:** Canonical Ltd.
**Empresas que lo utilizan:** Netflix, Wikipedia, Snapchat, eBay.

### Características Técnicas Fundamentales

Ubuntu Server es una distribución Linux derivada de Debian, optimizada específicamente para entornos de servidor. Su arquitectura está basada en el kernel Linux con optimizaciones para operaciones de entrada/salida intensivas, gestión de memoria en sistemas multiusuario y soporte nativo para tecnologías de virtualización como KVM y LXD.

El sistema utiliza **APT (Advanced Package Tool)** como gestor de paquetes, proporcionando acceso a más de 50,000 paquetes verificados y firmados criptográficamente.

Una característica diferenciadora es **Systemd** como sistema de inicialización. Systemd proporciona arranque paralelo de servicios, gestión avanzada de procesos (*cgroups*), *logging* centralizado con *journald* y control granular de recursos. Esto significa que todos los servicios del proyecto (Apache, MySQL, BIND9, ISC-DHCP, vsftpd) se gestionan con comandos uniformes como `systemctl start`, `systemctl stop` y `systemctl status`.

Las versiones **LTS (Long Term Support)** ofrecen 5 años de actualizaciones de seguridad garantizadas.

En cuanto a **seguridad**, Ubuntu Server incluye *AppArmor* habilitado por defecto y **UFW (Uncomplicated Firewall)** como interfaz simplificada para configurar el *firewall*.

### Por Qué Supera a Debian para Este Proyecto

* **Facilidad de instalación y configuración inicial:** Ubuntu Server simplifica el proceso con perfiles preconfigurados, reduciendo el tiempo de instalación de 30-35 minutos (Debian) a **15-20 minutos** (Ubuntu), lo cual es crucial para un plazo de 6 semanas.
* **Netplan como configurador de red moderno:** Utiliza archivos de configuración **YAML**, mucho más legible e intuitivo que `/etc/network/interfaces` de Debian. Incluye el comando `netplan try` que aplica una configuración temporalmente, evitando bloqueos accidentales.
* **Versiones de software más actualizadas:** Ubuntu Server **22.04 LTS** incluye **MySQL 8.0**, significativamente más reciente y eficiente (vital para la importación CSV del Sprint 2) que MySQL 5.7 en Debian Stable.
* **Documentación orientada a usuarios:** La documentación oficial de Ubuntu es más estructurada y orientada a guías paso a paso para usuarios con menos experiencia.
* **Compatibilidad con entornos virtualizados:** Optimizado y certificado para funcionar en plataformas como *IsardVDI*, incluyendo herramientas como *cloud-init*.

---

## 🗄️ MySQL - Sistema de Gestión de Bases de Datos Relacional

**Desarrollador:** Oracle Corporation
**Empresas que lo utilizan:** Meta (Facebook), YouTube, Booking.com, Twitter.

### Características Técnicas Fundamentales

MySQL es un sistema de gestión de bases de datos relacional (**RDBMS**). Su arquitectura *pluggable* permite usar diferentes motores, siendo **InnoDB** el motor por defecto que proporciona:

* Transacciones **ACID** completas.
* Bloqueo a nivel de fila (*row-level locking*) para alta concurrencia.
* **MVCC** (*Multi-Version Concurrency Control*).

**MySQL 8.0** introdujo mejoras como:
* Almacenamiento y consulta nativa de **JSON**.
* Mejoras en rendimiento y *Window functions*.

### Por Qué Supera a PostgreSQL para Este Proyecto

* **Importación directa de archivos CSV:** El comando nativo `LOAD DATA INFILE` es simple y eficiente para el requisito del Sprint 2:

    ```sql
    LOAD DATA LOCAL INFILE '/ruta/equipaments_educacio.csv'
    INTO TABLE equipaments
    FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    LINES TERMINATED BY '\n'
    IGNORE 1 ROWS;
    ```
    PostgreSQL requiere mayores restricciones de privilegios y configuraciones más complejas con `COPY`.
* **Integración nativa con PHP:** PHP incluye la extensión **mysqli** por defecto, simplificando el desarrollo del Sprint 3:
    ```php
    $conexion = mysqli_connect("192.168.30.4", "bchecker", "bchecker121", "educacio_bcn");
    $resultado = mysqli_query($conexion, "SELECT nom_equipament, adreca FROM equipaments");
    while($fila = mysqli_fetch_assoc($resultado)) {
        echo $fila['nom_equipament'] . " - " . $fila['adreca'];
    }
    ```
    PostgreSQL requiere instalar paquetes (`php-pgsql`) y usa funciones menos intuitivas.
* **Herramientas gráficas de administración:** **phpMyAdmin** proporciona una interfaz web extremadamente intuitiva para inspeccionar datos, esencial para la validación del Sprint 2.
* **Configuración de acceso remoto simplificada:** La configuración de acceso remoto para el entorno multicapa es directa:
    ```sql
    CREATE USER 'bchecker'@'%' IDENTIFIED BY 'bchecker121';
    GRANT ALL PRIVILEGES ON educacio_bcn.* TO 'bchecker'@'%';
    FLUSH PRIVILEGES;
    ```
    PostgreSQL requiere editar manualmente dos archivos de configuración (`postgresql.conf` y `pg_hba.conf`), un proceso más complejo.
* **Curva de aprendizaje optimizada:** MySQL permite enfocarse en conceptos fundamentales de bases de datos relacionales sin dispersarse en características avanzadas innecesarias para este proyecto.

---

## 🌐 Netplan - Configurador Moderno de Redes

**Desarrollador:** Canonical Ltd.
**Contexto de uso:** Estándar en Ubuntu Server, implementado en infraestructuras *cloud*.

### Características Técnicas Fundamentales

Netplan utiliza **configuración declarativa** mediante archivos **YAML** (`/etc/netplan/`) sobre el *backend* `systemd-networkd`.

El formato YAML ofrece legibilidad y facilita el **versionado** en Git. Su comando clave es `netplan try`, que aplica la configuración temporalmente (120 segundos) y revierte automáticamente si se pierde la conectividad, previniendo el bloqueo del sistema.

### Por Qué Supera a la Configuración Manual de Debian

* **Configuración de múltiples interfaces simplificada:** Para el router R-N03 con tres interfaces, Netplan usa un único archivo **YAML** legible:
    ```yaml
    network:
      version: 2
      renderer: networkd
      ethernets:
        enp0s3:
          dhcp4: true
        enp0s8:
          addresses: [192.168.130.1/24]
        enp0s9:
          addresses: [192.168.30.1/24]
    ```
    El método tradicional de Debian (`/etc/network/interfaces`) es más verboso y propenso a errores de sintaxis no detectados.
* **Integración con control de versiones Git:** Los *diffs* de los archivos YAML son claros y concisos, optimizando la auditoría y el *rollback* con Git.
* **Compatibilidad con entornos virtualizados (IsardVDI):** `netplan try` es fundamental para evitar la inaccesibilidad en máquinas virtuales sin consola física.

---

## 🌳 Git y GitHub - Control de Versiones Distribuido

**Desarrollador:** Software libre creado por **Linus Torvalds**. GitHub es propiedad de Microsoft Corporation.
**Empresas que lo utilizan:** Prácticamente el 100% de proyectos *open source* modernos.

### Características Técnicas Fundamentales

Git es un **DVCS (sistema de control de versiones distribuido)**. Cada repositorio contiene el historial completo del proyecto, permitiendo **trabajo offline**, **redundancia** y **velocidad**.

El **Branching y merging eficientes** es clave, permitiendo **desarrollo paralelo** seguro y la revisión de código mediante *Pull Requests* en **GitHub**.

### Por Qué Es Obligatorio y Sin Alternativas Viables

* **Requisito explícito del proyecto:** La documentación exige el uso de un repositorio Git.
* **Versionado de configuraciones de infraestructura:** Git registra cada modificación en configuraciones (Netplan, Apache, etc.), permitiendo la **trazabilidad completa** (`git log`) y la **recuperación ante errores** (`git checkout HEAD~1 -- configs/apache/000-default.conf`).
    ```bash
    git checkout HEAD~1 -- configs/apache/000-default.conf
    ```
* **Colaboración sin conflictos:** Resuelve la problemática del trabajo en equipo mediante *branches* y la **detección automática de conflictos**.
* **Auditoría para evaluación académica:** Permite al profesor evaluar contribuciones individuales objetivamente (`git log --author="NombreEstudiante"`).

---

## 🐘 Apache HTTP Server - Servidor Web

**Desarrollador:** Apache Software Foundation
**Empresas que lo utilizan:** Adobe, LinkedIn, Cisco, la mayoría de proveedores de hosting compartido.

### Características Técnicas Fundamentales

Apache tiene una **arquitectura modular** (módulos como `mod_php`, `mod_rewrite`, `mod_ssl`) y soporta diferentes **MPM (Multi-Processing Modules)**.

Una característica única es el soporte para archivos **.htaccess**, que permiten **configuración descentralizada** a nivel de directorio (reescritura de URLs, restricciones de acceso) sin reiniciar el servidor ni requerir permisos de *root*.

La **integración directa con PHP mediante `mod_php`** permite que el código PHP se ejecute dentro del proceso de Apache sin servicios adicionales, simplificando el ciclo de desarrollo para el Sprint 3.

### Por Qué Supera a Nginx para Este Proyecto

* **Despliegue de aplicación PHP sin configuración compleja:** Con Apache, PHP funciona inmediatamente tras la instalación de `libapache2-mod-php`. **Nginx** requiere instalar y configurar **PHP-FPM** como servicio separado, introduciendo conceptos adicionales (Sockets Unix, FastCGI) que aumentan la complejidad y el tiempo de *troubleshooting*.
    Configuración de Nginx (compleja):
    ```nginx
    server {
        listen 80;
        root /var/www/html;
        index index.php index.html;
        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        }
    }
    ```
* ***Troubleshooting* simplificado:** Apache centraliza los errores de PHP en `/var/log/apache2/error.log`, mostrando la línea exacta del fallo. Nginx fragmenta el diagnóstico en múltiples logs (`nginx/error.log` y `php-fpm.log`).
* **Archivos .htaccess para configuración flexible:** Apache permite cambios rápidos sin reiniciar. **Nginx no soporta `.htaccess`**, requiriendo editar la configuración principal (`sudo`) y reiniciar el servicio para cualquier modificación.
* **Documentación abundante en español:** El *stack* LAMP tiene una base de conocimientos mucho más amplia y accesible para estudiantes.
* **Tiempo de implementación del Sprint 3:** Apache requiere **~30 minutos** de configuración; Nginx puede extenderse a **1.5-2 horas**, restando tiempo al desarrollo y la documentación evaluada.
* **Rendimiento suficiente:** Para la baja carga del proyecto, el rendimiento de Apache es más que suficiente, haciendo que la complejidad de Nginx no se justifique.

---

## ✨ Sinergias Entre Tecnologías Seleccionadas

Las tecnologías elegidas forman un **ecosistema integrado** que optimiza el desarrollo.

### Stack LAMP Clásico Actualizado

La combinación forma el *stack* LAMP (Linux + Apache + MySQL + PHP), complementado con Git y Netplan:

* **Instalación homogénea mediante APT:** Todas las tecnologías se instalan con un único gestor de paquetes y un solo comando, garantizando versiones compatibles.
    ```bash
    apt install apache2 mysql-server php libapache2-mod-php php-mysql bind9 isc-dhcp-server vsftpd git
    ```
* **Gestión unificada mediante Systemd:** Todos los servicios se controlan con comandos idénticos (`systemctl start/status/restart`) y logs centralizados en *journald*.
* **Flujo de datos integrado:** Cada transición (CSV → MySQL → PHP → Apache) es **nativa**, sin *adapters* o servicios intermedios.
* **Configuraciones versionables en Git:** Todas las configuraciones son archivos de texto plano que permiten **infraestructura como código**, despliegue reproducible y auditoría completa.

### Curva de Aprendizaje Incremental

El proyecto está diseñado para construir conocimiento progresivamente:

| Sprint | Enfoque Principal | Tecnologías y Conceptos |
| :--- | :--- | :--- |
| **Sprint 1** | Redes | Netplan, Git, conceptos de redes, gestión básica de Linux. |
| **Sprint 2** | Servicios | MySQL, DHCP, DNS, importación CSV, uso de Systemd. |
| **Sprint 3** | Aplicación Web | PHP, conexión a bases de datos, HTML/CSS, arquitectura multicapa. |

---

## ✅ Conclusión

La selección de tecnologías (Ubuntu Server, MySQL, Netplan, Git/GitHub y Apache) no es arbitraria, sino resultado de un análisis técnico que prioriza:

1.  **Cumplimiento de requisitos obligatorios:** Satisfacen todos los puntos explícitos del proyecto.
2.  **Optimización temporal:** Minimizan la complejidad innecesaria (ej. Apache vs Nginx), destinando el tiempo ganado al aprendizaje conceptual y la documentación.
3.  **Facilidad de aprendizaje:** Curva progresiva, documentación abundante y ejemplos educativos.
4.  **Preparación profesional:** El *stack* LAMP actualizado y el dominio de Git son habilidades altamente demandadas.

Las alternativas analizadas (Debian, PostgreSQL, Nginx) son tecnologías excelentes, pero introducen complejidad adicional que no se traduce en beneficios proporcionales para los requisitos específicos y el corto plazo (6 semanas) de este proyecto académico.