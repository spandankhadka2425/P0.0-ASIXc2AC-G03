# Guía Completa: Configuración de Servidor Web Apache

Esta guía explica cómo configurar la red, instalar Apache2, crear un usuario, y verificar el funcionamiento de un servidor web en Linux.

---

## 1. Configuración de IP

Edita la configuración de red para asignar una IP estática:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Ejemplo de configuración en YAML:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
       dhcp4: true
    enp2s0:
      addresses:
        - 192.168.130.2/24
      routes:
        - to: default
          via: 192.168.130.1  
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

Aplica los cambios:

```bash
sudo netplan apply
```

---

## 2. Instalación de Apache2

Actualiza el sistema e instala Apache2:

```bash
sudo apt update && sudo apt install apache2 -y
```

Inicia y habilita el servicio:

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

## 3. Creación de Usuario `bchecker`

Crea el usuario y asigna contraseña:

```bash
sudo useradd -m -s /bin/bash bchecker
echo "bchecker:bchecker121" | sudo chpasswd
```

Agrega el usuario al grupo sudo:

```bash
sudo usermod -aG sudo bchecker
```

---

## 4. Verificación del Servicio Web

Verifica que Apache está corriendo:

```bash
sudo systemctl status apache2
```

Comprueba que responde en la IP configurada:

```bash
curl -I http://192.168.130.2
```

---

## 5. Modificar la Página Principal

Edita el archivo principal del sitio web:

```bash
sudo nano /var/www/html/index.html
```

---

## 6. Verificación de Archivos del Sitio Web

Lista los archivos del directorio web:

```bash
ls -la /var/www/html/
```

Verifica el contenido actual del index:

```bash
cat /var/www/html/index.html
```

---

## Explicación

- **Configuración de IP:** Se edita el archivo de configuración de red para asignar una IP estática y definir rutas y DNS.
- **Instalación de Apache2:** Se instala el servidor web Apache y se asegura que esté activo y se inicie automáticamente.
- **Usuario bchecker:** Se crea un usuario específico para tareas administrativas y se le da acceso sudo.
- **Verificación:** Se comprueba que el servicio web está activo y responde correctamente en la IP configurada.
- **Modificación del index:** Permite editar la página principal del sitio web.
- **Verificación de archivos:** Se revisa el contenido y los archivos del directorio web para asegurar que todo está correcto.

Esta guía te ayuda a tener un servidor web Apache funcional y seguro en tu