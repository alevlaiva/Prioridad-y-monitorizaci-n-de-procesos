
# UD 03.04 - Caso práctico 02  
## Instalando LAMP + WordPress en un contenedor


## 1. Introducción

En este caso práctico se utiliza como base la imagen oficial de **Ubuntu**, sobre la que se instalará  
Apache, PHP y MySQL (LAMP), junto con el CMS **WordPress**.

---

## 2. Preparando el contenedor

Se crea un contenedor a partir de la imagen `ubuntu:22.10`, exponiendo el puerto 80 del contenedor  
en el puerto 8080 del sistema anfitrión.

```bash
docker run -it -p 8080:80 --name LAMP ubuntu:22.10 /bin/bash
```

Al ejecutarlo correctamente, se accede a la shell del contenedor.

---

## 3. Instalando LAMP y WordPress

### 3.1 Actualizando repositorio e instalando LAMP + WordPress

Actualizar repositorios:

```bash
apt update
```

Instalar paquetes necesarios:

```bash
apt install wordpress php libapache2-mod-php mariadb-server php-mysql
```

Iniciar Apache:

```bash
service apache2 start
```

Comprobación desde el navegador:

```
http://localhost:8080
```

---

### 3.2 Preparando Apache para usar WordPress

Instalar editor de texto (nano):

```bash
apt install nano
```

Crear el fichero de configuración:

```bash
nano /etc/apache2/sites-available/wordpress.conf
```

Contenido:

```apache
Alias /blog /usr/share/wordpress

<Directory /usr/share/wordpress>
    Options FollowSymLinks
    AllowOverride Limit Options FileInfo
    DirectoryIndex index.php
    Order allow,deny
    Allow from all
</Directory>

<Directory /usr/share/wordpress/wp-content>
    Options FollowSymLinks
    Order allow,deny
    Allow from all
</Directory>
```

Activar sitio y módulos:

```bash
a2ensite wordpress
a2enmod rewrite
service apache2 restart
```

Acceso:

```
http://localhost:8080/blog
```

---

### 3.3 Preparando MariaDB Server

Iniciar el servicio:

```bash
service mariadb start
```

Configuración segura:

```bash
mysql_secure_installation
```

Acceder a MariaDB:

```bash
mysql -u root -p
```

Crear base de datos:

```sql
CREATE DATABASE wordpress;
```

Crear usuario y permisos:

```sql
CREATE USER 'wordpress'@'%' IDENTIFIED BY 'MiPass-2023';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

### 3.4 Configurando WordPress

Editar el fichero:

```bash
nano /etc/wordpress/config-localhost.php
```

Contenido:

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'MiPass-2023');
define('DB_HOST', 'localhost');
define('DB_COLLATE', 'utf8_general_ci');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

---

### 3.5 Configurando el sitio desde el navegador

Acceder a:

```
http://localhost:8080/blog
```

Completar la configuración inicial de WordPress.

---

### 3.6 Arranque automático de servicios

Editar el fichero `.bashrc`:

```bash
nano /root/.bashrc
```

Añadir al final:

```bash
service apache2 start
service mariadb start
```

Esto permite que los servicios se inicien al arrancar el contenedor.

---

### 3.7 Comprobaciones finales

Reiniciar el contenedor:

```bash
docker stop LAMP
docker start LAMP
```

Comprobar acceso:

```
http://localhost:8080/blog
```

---

