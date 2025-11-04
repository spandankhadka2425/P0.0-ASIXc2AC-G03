# Guía Completa: Instalación y Verificación de MySQL con Usuario `bchecker`

## Instalación de MySQL Server

```bash
# Actualiza el sistema e instala MySQL Server
sudo apt update && sudo apt install mysql-server -y

# Inicia y habilita el servicio MySQL
sudo systemctl start mysql
sudo systemctl enable mysql
```

## Asegura la Instalación

```bash
# Ejecuta el script de seguridad de MySQL
sudo mysql_secure_installation
```

## Acceso a MySQL como root

```bash
sudo mysql -u root -p
```

---

## 1 Verificar que la Base de Datos Existe

```sql
-- Listar todas las bases de datos
SHOW DATABASES;

-- Verificar que existe barcelona_education
SELECT SCHEMA_NAME FROM information_schema.SCHEMATA WHERE SCHEMA_NAME = 'barcelona_education';
```

## 2 Verificar el Usuario `bchecker`

```sql
-- Listar todos los usuarios
SELECT user, host FROM mysql.user;

-- Verificar el usuario bchecker
SELECT user, host, authentication_string FROM mysql.user WHERE user = 'bchecker';
```

## 3 Verificar Permisos del Usuario

```sql
-- Verificar permisos de bchecker
SHOW GRANTS FOR 'bchecker'@'localhost';
```

## 4 Acceder con el Usuario `bchecker`

```bash
# Salir de MySQL y acceder con bchecker
exit
mysql -u bchecker -p barcelona_education
```

## 5 Verificar las Tablas

```sql
-- Listar todas las tablas
SHOW TABLES;

-- Verificar la estructura de la tabla equipaments
DESCRIBE equipaments;

-- Ver la estructura completa de la tabla
SHOW CREATE TABLE equipaments;
```

## 6 Verificar los Datos Cargados

```sql
-- Contar el total de registros
SELECT COUNT(*) AS total_registros FROM equipaments;

-- Ver muestras de datos (primeras 5 filas)
SELECT * FROM equipaments LIMIT 5;

-- Contar registros por distrito
SELECT addresses_district_name, COUNT(*) AS cantidad 
FROM equipaments 
GROUP BY addresses_district_name 
ORDER BY cantidad DESC;

-- Contar categorías únicas
SELECT values_category, COUNT(*) AS cantidad 
FROM equipaments 
GROUP BY values_category 
ORDER BY cantidad DESC;

-- Ver algunos nombres de institutos
SELECT name, addresses_district_name, values_category 
FROM equipaments 
LIMIT 10;
```

## 7 Verificar Tipos de Datos y Valores

```sql
-- Verificar rango de valores numéricos
SELECT 
    MIN(id) AS min_id,
    MAX(id) AS max_id,
    COUNT(DISTINCT register_id) AS ids_unicos
FROM equipaments;

-- Verificar valores únicos importantes
SELECT COUNT(DISTINCT name) AS nombres_unicos FROM equipaments;
SELECT COUNT(DISTINCT addresses_district_name) AS distritos_unicos FROM equipaments;
SELECT COUNT(DISTINCT addresses_neighborhood_name) AS barrios_unicos FROM equipaments;
```