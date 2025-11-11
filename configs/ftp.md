# Guía Completa: Instalación y Configuración de FTP con vsftpd

Esta guía te muestra cómo instalar y configurar un servidor FTP seguro usando vsftpd en Linux, crear un usuario, ajustar la configuración y abrir los puertos necesarios en el firewall.

---

## 1. Creación de Usuario `bchecker`

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

## 2. Instalación de vsftpd

Actualiza el sistema e instala vsftpd:

```bash
sudo apt update && sudo apt install vsftpd -y
```

Inicia y habilita el servicio:

```bash
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

---

## 3. Configuración de vsftpd

Haz una copia de seguridad del archivo de configuración original:

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.backup
```

Edita el archivo de configuración:

```bash
sudo nano /etc/vsftpd.conf
```

Configuración recomendada para `/etc/vsftpd.conf`:

```
write_enable=YES
local_enable=YES
anonymous_enable=NO
local_root=/home/$USER
chroot_local_user=YES
allow_writeable_chroot=YES
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=31000
xferlog_enable=YES
xferlog_file=/var/log/vsftpd.log
```

---

## 4. Reiniciar y Verificar el Servicio

Reinicia el servicio para aplicar los cambios:

```bash
sudo systemctl restart vsftpd
```

Verifica el estado del servicio:

```bash
sudo systemctl status vsftpd
```

Comprueba que vsftpd está escuchando en el puerto 21:

```bash
sudo netstat -tulpn | grep :21
```

---

## 5. Configuración de Firewall para FTP

Abre los puertos necesarios en el firewall:

```bash
sudo ufw allow 21/tcp
sudo ufw allow 20/tcp
sudo ufw allow 30000:31000/tcp
```

Verifica las reglas del firewall:

```bash
sudo ufw status
```

---

## Explicación

- **Usuario bchecker:** Se crea un usuario administrativo para acceder al sistema y gestionar archivos vía FTP.
- **Instalación de vsftpd:** Se instala el servidor FTP y se asegura que esté activo y se inicie automáticamente.
- **Configuración de vsftpd:** Se ajusta el archivo de configuración para permitir usuarios locales, deshabilitar el acceso anónimo, definir el directorio raíz y habilitar el modo pasivo.
- **Reinicio y verificación:** Se reinicia el servicio para aplicar los cambios y se verifica que el servidor FTP está funcionando correctamente.
- **Firewall:** Se abren los puertos necesarios para el funcionamiento del FTP y se comprueba que las reglas estén activas.

Esta guía te ayuda a tener un servidor FTP funcional y seguro en tu sistema