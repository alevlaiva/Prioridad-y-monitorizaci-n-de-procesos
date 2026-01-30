# Introducción a Docker  
## UD 05. Caso práctico 01 – Wordpress + MariaDB


## Contenido
1. Introducción  
2. Paso 1: Creando la red  
3. Paso 2: Creando contenedor MariaDB  
4. Paso 3: Creando contenedor con Wordpress  
5. Paso 4: Instalando Wordpress  
6. Migrando contenedor MariaDB de 10.6 a 10.7  

---

## 1. Introducción
En este caso práctico vamos a poner en marcha el popular CMS **Wordpress** usando Docker.
Se crearán dos contenedores conectados por una red:
- Apache + PHP + Wordpress
- MariaDB

También se realizará una migración de versión de MariaDB.

---

## 2. Paso 1: Creando la red

```bash
docker network create redwp
```

---

## 3. Paso 2: Creando contenedor MariaDB

```bash
docker run --name nuestromariadb --network redwp \
-v /home/sergi/mariadbdata:/var/lib/mysql \
-e MARIADB_ROOT_PASSWORD=cefireroot \
-e MARIADB_USER=cefireuser \
-e MARIADB_PASSWORD=cefirepass \
-e MARIADB_DATABASE=cefiredb \
-d mariadb:10.6
```

Se crea un contenedor MariaDB con:
- Usuario y contraseña personalizados
- Base de datos inicial
- Persistencia mediante *binding mount*

---

## 4. Paso 3: Creando contenedor con Wordpress

```bash
docker run --name nuestrowp --network redwp -p 8080:80 -d wordpress
```

El puerto 80 del contenedor se expone en el puerto 8080 del host.

---

## 5. Paso 4: Instalando Wordpress

Acceder desde el navegador a:

```
http://localhost:8080
```

Configurar la base de datos usando:
- Host: `nuestromariadb`
- Usuario, contraseña y base de datos definidos en MariaDB

---

## 6. Migrando contenedor MariaDB de 10.6 a 10.7

Detener y eliminar el contenedor antiguo:

```bash
docker stop nuestromariadb
docker rm nuestromariadb
```

Crear el nuevo contenedor usando los mismos datos persistentes:

```bash
docker run --name nuestromariadb --network redwp \
-v /home/sergi/mariadbdata:/var/lib/mysql \
-e MARIADB_ROOT_PASSWORD=cefireroot \
-e MARIADB_USER=cefireuser \
-e MARIADB_PASSWORD=cefirepass \
-e MARIADB_DATABASE=cefiredb \
-d mariadb:10.7
```

Wordpress seguirá funcionando sin pérdida de datos.

