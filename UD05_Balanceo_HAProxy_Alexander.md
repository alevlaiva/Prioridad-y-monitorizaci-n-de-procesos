# Introducción a Docker  
## UD 05. Caso práctico 02 – Balanceo de carga con HAProxy



## Contenido
1. Introducción  
2. Paso 1: Creando las imágenes necesarias  
   1. Dockerfile para imágenes de servidores Apache  
   2. Dockerfile para imagen de servidor HAProxy  
3. Paso 2: Creando la red y los contenedores con Apache  
4. Paso 3: Creando contenedor con HAProxy  
5. Servidores de Apache usando un volumen común  




## 1. Introducción
En este caso práctico instalaremos un contenedor Docker con **HAProxy** que realizará balanceo de carga
entre dos contenedores con servidores **Apache** independientes.  
Posteriormente, modificaremos la práctica para que ambos servidores lean datos desde un mismo volumen.

---

## 2. Paso 1: Creando las imágenes necesarias

Se crean dos Dockerfiles para Apache y uno para HAProxy.

### Construcción de imágenes

```bash
cd apache01
docker build -t miapache01 .

cd ../apache02
docker build -t miapache02 .

cd ../haproxy
docker build -t mihaproxy .
```

---

## 2.1 Dockerfile para imágenes de servidores Apache

Basadas en la imagen oficial:

https://hub.docker.com/_/httpd

```dockerfile
FROM httpd:2.4
COPY index.html /usr/local/apache2/htdocs/index.html
```

Cada imagen difiere en el contenido del `index.html`.

---

## 2.2 Dockerfile para imagen de servidor HAProxy

Basada en:

https://hub.docker.com/_/haproxy

```dockerfile
FROM haproxy:1.7
COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
```

### Configuración principal de HAProxy

- Escucha en el puerto 80
- Balanceo **roundrobin**
- Servidores definidos mediante variables de entorno

```cfg
server apache1 ${APACHE_1_IP}:${APACHE_EXPOSED_PORT} check
server apache2 ${APACHE_2_IP}:${APACHE_EXPOSED_PORT} check
```

---

## 3. Paso 2: Creando la red y los contenedores con Apache

```bash
docker network create balanceo

docker run -it --name ap01 --network balanceo -d miapache01
docker run -it --name ap02 --network balanceo -d miapache02
```

---

## 4. Paso 3: Creando contenedor con HAProxy

```bash
docker run -it --name ha01 --network balanceo -p 8080:80 \
-e APACHE_1_IP=ap01 \
-e APACHE_2_IP=ap02 \
-e APACHE_EXPOSED_PORT=80 \
-d mihaproxy
```

Acceso desde el host:

```
http://localhost:8080
```

Cada recarga será atendida por un servidor diferente.

---

## 5. Servidores de Apache usando un volumen común

Construimos una nueva imagen:

```bash
cd apachevolumen
docker build -t miapachevolumen .
```

Eliminamos contenedores anteriores:

```bash
docker stop ap01 ap02
docker rm ap01 ap02
```

Creamos nuevos contenedores usando un volumen compartido:

```bash
docker run -it --name ap01 --network balanceo -v datosapache:/usr/local/apache2/htdocs/ -d miapachevolumen

docker run -it --name ap02 --network balanceo -v datosapache:/usr/local/apache2/htdocs/ -d httpd:2.4
```

Ambos servidores sirven el mismo contenido desde el volumen.

---

