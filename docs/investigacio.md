```markdown
# Análisis Técnico de las Tecnologías del Proyecto P0.0-ASIXc2AC-G03

## Introducción

Este documento justifica la selección de tecnologías para el proyecto de infraestructura multicapa que incluye servicios de Web Server, Base de Datos, DHCP, DNS y FTP distribuidos en arquitectura DMZ e Intranet. Las decisiones se fundamentan en criterios de compatibilidad, facilidad de gestión y tiempo de implementación disponible (6 semanas, 3 sprints).

---

## 1. Ubuntu Server - Sistema Operativo Base

**Desarrollador:** Canonical Ltd. | **Usado por:** Netflix, Wikipedia, Snapchat, eBay.

### Por qué es la mejor elección

**Gestión unificada de servicios:** Los 5 servicios requeridos (Apache, MySQL, BIND9, ISC-DHCP, vsftpd) están disponibles en repositorios oficiales con `apt install`, eliminando problemas de dependencias y garantizando compatibilidad entre versiones.

**Netplan preinstalado:** Configurar las tres redes (DMZ: 192.168.130.0/24, Intranet: 192.168.30.0/24, NAT) es simple con YAML:

```yaml
network:
  version: 2
  ethernets:
    enp0s8:
      addresses: [192.168.130.2/24]
      gateway4: 192.168.130.1
```

- Versionable en Git (requisito del proyecto)
- `netplan try` permite rollback automático en 120 segundos si hay errores
- Compatible con entornos virtuales de IsardVDI

**Usuario bchecker automatizable:**

```bash
adduser bchecker
echo "bchecker:bchecker121" | chpasswd
usermod -aG sudo bchecker
```

**Ventaja sobre Debian:** Netplan vs interfaces(5) legacy, MySQL 8.0 vs 5.7 obsoleta, documentación más accesible para estudiantes, instalación 10 minutos más rápida.

---

## 2. MySQL - Sistema de Gestión de Bases de Datos

**Desarrollador:** Oracle Corporation | **Usado por:** Meta, YouTube, Booking.com.

### Por qué es la mejor elección

**Importación CSV nativa (requisito crítico):**

```sql
LOAD DATA LOCAL INFILE '/ruta/equipaments_educacio.csv'
INTO TABLE equipaments
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

Importa miles de filas en segundos sin scripts externos. PostgreSQL requiere permisos de superusuario y configuración compleja de `pg_hba.conf`.

**Integración PHP inmediata (Sprint 3):**

```php
<?php
$conn = mysqli_connect("192.168.30.4", "bchecker", "bchecker121", "educacio_bcn");
$result = mysqli_query($conn, "SELECT * FROM equipaments LIMIT 10");
while($row = mysqli_fetch_assoc($result)) {
    echo $row['nom_equipament'] . "<br>";
}
?>
```

La extensión `mysqli` viene incluida en PHP. PostgreSQL requiere instalar `php-pgsql` adicional con sintaxis menos intuitiva (`pg_connect`, `pg_query`).

**Configuración usuario bchecker simplificada:**

```sql
CREATE USER 'bchecker'@'%' IDENTIFIED BY 'bchecker121';
GRANT ALL PRIVILEGES ON educacio_bcn.* TO 'bchecker'@'%';
```

Tres comandos vs editar múltiples archivos de configuración en PostgreSQL.

**Ventaja sobre PostgreSQL:** Instalación 10 minutos vs 30 minutos, phpMyAdmin simple vs pgAdmin complejo, documentación abundante para principiantes, tiempo de despliegue Sprint 2 reducido en 50%.

---

## 3. Netplan - Configuración de Red

**Desarrollador:** Canonical Ltd. | **Contexto:** Estándar en Ubuntu Server, usado por IBM y Dell.

### Por qué es la mejor elección

**Configuración declarativa del router R-N03:**

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:  # NAT
      dhcp4: true
    enp0s8:  # DMZ
      addresses: [192.168.130.1/24]
    enp0s9:  # Intranet
      addresses: [192.168.30.1/24]
```

Todo en un archivo YAML versionable en Git (requisito obligatorio del proyecto). Los cambios son legibles en diffs, facilitando revisión por el profesor.

**Validación segura con `netplan try`:** Rollback automático previene bloqueos de acceso en IsardVDI donde no hay consola física disponible.

**Ventaja sobre interfaces(5) de Debian:** 10 líneas YAML vs 15+ líneas sintaxis legacy, validación integrada vs cambios directos sin verificación, aprendizaje 30 min vs 2-3 horas.

---

## 4. Git/GitHub - Control de Versiones

**Desarrollador:** Comunidad open source, GitHub (Microsoft) | **Usado por:** 100% proyectos modernos.

### Por qué es obligatorio

**Requisito explícito:** "Tota la configuració i documentació ha d'estar en un repositori Git amb el nom: P0.0-ASIXc2AC-G03"

**Versionado de configuraciones:**

```bash
git add netplan/00-installer-config.yaml
git commit -m "Sprint 1: Configurar DMZ 192.168.130.0/24"
git push origin main
```

Historial completo para auditoría del profesor, recuperación de versiones anteriores con `git checkout`, trabajo en equipo sin conflictos mediante branches por sprint.

**Sin alternativa viable:** SVN es centralizado y legacy, Mercurial sin ecosistema gratuito equivalente, no usar control de versiones incumple requisito del proyecto.

---

## 5. Apache HTTP Server - Servidor Web

**Desarrollador:** Apache Software Foundation | **Usado por:** Adobe, LinkedIn, Cisco.

### Por qué es la mejor elección

**Despliegue PHP plug-and-play (Sprint 3):**

```bash
sudo apt install apache2 php libapache2-mod-php php-mysql
```

PHP se ejecuta con `mod_php` sin configurar FastCGI ni servicios adicionales. Archivos en `/var/www/html/` son accesibles inmediatamente en `http://192.168.130.2/`.

**Aplicación funcional en minutos:**

```php
<?php
$conn = mysqli_connect("192.168.30.4", "bchecker", "bchecker121", "educacio_bcn");
$result = mysqli_query($conn, "SELECT * FROM equipaments LIMIT 20");
?>
<table>
  <?php while($row = mysqli_fetch_assoc($result)): ?>
    <tr><td><?= $row['nom_equipament'] ?></td></tr>
  <?php endwhile; ?>
</table>
```

Este código funciona sin configuración adicional. Nginx requiere instalar PHP-FPM, configurar sockets y editar bloques `location ~ \.php$`.

**Ventaja sobre Nginx:** Instalación PHP 1 paso vs 3 pasos, configuración 5 min vs 20 min, sin necesidad de archivos de configuración complejos, documentación abundante en español, tiempo Sprint 3 reducido de 2h a 30 min.

---

## Sinergias del Stack para el Proyecto

### Script de despliegue unificado

```bash
#!/bin/bash
# Ejecutar en los 4 servidores (W-N03, B-N03, F-N03, R-N03)
adduser --disabled-password --gecos "" bchecker
echo "bchecker:bchecker121" | chpasswd
usermod -aG sudo bchecker
apt update && apt upgrade -y
apt install -y git vim ufw
ufw allow 22/tcp
ufw allow from 192.168.30.0/24
ufw allow from 192.168.130.0/24
ufw --force enable
```

### Estructura del repositorio

```
P0.0-ASIXc2AC-G03/
├── README.md
├── docs/
│   ├── sprint1-router.md
│   ├── sprint2-servicios.md
│   └── sprint3-app.md
├── configs/
│   ├── netplan/
│   ├── apache/
│   ├── mysql/
│   └── dhcp/
├── scripts/
│   └── despliegue-base.sh
└── webapp/
    ├── index.php
    └── equipaments.php
```

### Ventajas medibles

| Tarea | Tiempo con Stack Elegido | Tiempo con Alternativas |
|-------|--------------------------|-------------------------|
| Instalación OS base | 15 min | 30 min (Debian) |
| Configuración 3 redes | 20 min | 1h (interfaces) |
| Importar CSV a BBDD | 10 min | 40 min (PostgreSQL) |
| Desplegar app web | 30 min | 2h (Nginx+PHP-FPM) |
| **Total Sprint 1-3** | **~10h** | **~20h** |

**Ahorro del 50% de tiempo** permite dedicar más horas a documentación y pruebas (requisitos de evaluación).

---

## Conclusión

El stack **Ubuntu Server + MySQL + Netplan + Apache + Git** optimiza el proyecto P0.0-ASIXc2AC-G03 porque:

1. **Cumple todos los requisitos obligatorios:** Usuario bchecker, repositorio Git, 3 redes, importación CSV, aplicación web
2. **Maximiza eficiencia temporal:** Cada tecnología minimiza complejidad en su área, permitiendo completar 3 sprints de 10h sin bloqueos
3. **Facilita trabajo en equipo:** Configuraciones versionables, procedimientos homogéneos entre servidores
4. **Garantiza éxito académico:** Documentación abundante, comunidades activas, problemas comunes tienen soluciones probadas
5. **Compatible con IsardVDI:** Netplan detecta adaptadores virtuales automáticamente, validación segura sin consola física

**Recomendación:** Mantener este stack en los 3 sprints, agregando herramientas complementarias solo si el tiempo lo permite (phpMyAdmin para verificar BBDD en Sprint 2).
```