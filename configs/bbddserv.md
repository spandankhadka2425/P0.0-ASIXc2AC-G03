
## Creando useario necessario 

```bash
sudo adduser bchecker
```

## Anadir el usuario en grupo de sudors 

```bash
 sudo usermod -aG sudo bchecker

```
## camprobar si pueder hacer login 
```bash
su bchecker
```

## cambiar el hostname a B-N03
```bash
sudo nano /etc/hostname 
```

# Configuracion de IP
```bash
 sudo nano /etc/netplan/00-installer-config.yaml 
```

```
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
       dhcp4: true
    enp2s0:
      addresses:
        - 192.168.DMZ.x/24
      routes:
        - to: default
          via: 192.168.130.1  
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

```

# Guía Completa: Instalación y Verificación de MySQL con Usuario `bchecker`

## Instalación de MySQL Server

```bash
# 
sudo apt update && sudo apt install mysql-server -y

# 
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Asegura la Instalación

```bash
# 
sudo mysql_secure_installation
```

## Acceso a MySQL como root

```bash
sudo mysql -u root -p
```

---

## 1 Verificar que la Base de Datos Existe

```sql
--
SHOW DATABASES;

-- 
SELECT SCHEMA_NAME FROM information_schema.SCHEMATA WHERE SCHEMA_NAME = 'barcelona_education';
```

## 2 Verificar el Usuario `bchecker`

```sql
-- 
SELECT user, host FROM mysql.user;

--
SELECT user, host, authentication_string FROM mysql.user WHERE user = 'bchecker';
```

## 3 Verificar Permisos del Usuario

```sql
-- 
SHOW GRANTS FOR 'bchecker'@'localhost';
```

## 4 Acceder con el Usuario `bchecker`

```bash
# 
exit
mysql -u bchecker -p barcelona_education
```

## 5 Verificar las Tablas

```sql
-- 
SHOW TABLES;

-- 
DESCRIBE equipaments

-- 
SHOW CREATE TABLE equipaments;
```

## 6 Verificar los Datos Cargados

```sql
-- 
SELECT COUNT(*) AS total_registros FROM equipaments;

-- 
SELECT * FROM equipaments LIMIT 5;

-- 
SELECT addresses_district_name, COUNT(*) AS cantidad 
FROM equipaments 
GROUP BY addresses_district_name 
ORDER BY cantidad DESC;

-- 
SELECT values_category, COUNT(*) AS cantidad 
FROM equipaments 
GROUP BY values_category 
ORDER BY cantidad DESC;

-- 
SELECT name, addresses_district_name, values_category 
FROM equipaments 
LIMIT 10;
```

## 7 Verificar Tipos de Datos y Valores

```sql
-- 
SELECT 
    MIN(id) AS min_id,
    MAX(id) AS max_id,
    COUNT(DISTINCT register_id) AS ids_unicos
FROM equipaments;

-- 
SELECT COUNT(DISTINCT name) AS nombres_unicos FROM equipaments;
SELECT COUNT(DISTINCT addresses_district_name) AS distritos_unicos FROM equipaments;
SELECT COUNT(DISTINCT addresses_neighborhood_name) AS barrios_unicos FROM equipaments;
```
## Carga de Datos CSV: Listado de equipamientos de educación de la ciudad de Barcelona

A continuación se presentan los enlaces al archivo CSV con el listado de equipamientos educativos de Barcelona:

- [Descargar CSV directo](https://opendata-ajuntament.barcelona.cat/data/dataset/f36b60f2-9541-4d08-b0f9-b0a9313fab3d/resource/29d9ff10-6892-4f16-9012-d5c4997857e7/download)

**Instrucción:**  
Carga estos datos en la base de datos creada para poder realizar las consultas y verificaciones correspondientes.

**Para aceder los datos via  web**  
```
http://http://192.168.130.3/CSV.php
```
