## UD 01.01 - Introducción a los contenedores y a Docker

###  Conceptos básicos

- **Virtualización:** Permite ejecutar sistemas virtuales sobre hardware físico.  
- **Máquinas virtuales:** Simulan sistemas completos mediante hipervisores (VirtualBox, VMware, etc.).  
- **Contenedores:** Entornos aislados a nivel de sistema operativo (OS-level virtualization).  
  - Comparten el kernel del sistema anfitrión.  
  - Son más ligeros y rápidos que las VMs.  

###  Ventajas de los contenedores

- Bajo consumo de recursos.  
- Mayor velocidad de ejecución.  
- Compatibilidad entre entornos (evita el “en mi máquina funciona…”).  
- Escalabilidad horizontal y soporte en entornos CI/CD.  

###  Contenedores en Linux

- Antecedentes: `chroot` (1982) y `jail` (FreeBSD, 1999).  
- Modernos: LXC, LXD, LXCFS.  
- Tecnologías clave:
  - **Namespaces:** Aislamiento de procesos y recursos.  
  - **Cgroups:** Limitación de recursos asignados.  

###  Docker y su arquitectura

- Sistema de contenedores basado en Linux.  
- Versiones:
  - **Docker CE:** gratuita y open source.  
  - **Docker EE:** versión empresarial con soporte oficial.  
- Compatible con **Windows (WSL2 / Hyper-V)** y **MacOS (Hyperkit)**.  

**Arquitectura básica:**
| Componente | Descripción |
|-------------|--------------|
| **Cliente** | Interfaz para comunicarse con el daemon. |
| **Servidor (Host)** | Gestiona imágenes y contenedores. |
| **Registry** | Repositorio de imágenes (por defecto, [Docker Hub](https://hub.docker.com)). |

# Docker - Instalación y Principales Acciones

##  UD 02.01 – Instalación de Docker


---

###  Instalación en Linux (Ubuntu)
####  Instalación recomendada (desde repositorio oficial de Docker CE)
1. **Eliminar versiones antiguas:**
   ```bash
   sudo apt-get remove docker docker-engine docker.io containerd runc
   ```
2. **Añadir repositorio de Docker CE:**
   ```bash
   sudo apt-get update
   sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
   sudo mkdir -m 0755 -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]    https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
3. **Instalar Docker CE:**
   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
4. **Comprobar instalación:**
   ```bash
   sudo docker version
   ```

---

###  Postinstalación
1. **Usar Docker sin permisos de root:**
   ```bash
   sudo groupadd docker
   sudo usermod -aG docker $USER
   ```
2. **Activar/desactivar arranque automático:**
   ```bash
   sudo systemctl enable|disable docker.service
   sudo systemctl enable|disable containerd.service
   ```

---

### Desinstalar Docker
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker system prune -a
sudo rm -rf /var/lib/docker
```

---

### Instalación en Windows
- **Windows 10 Pro/Server:** activar *Hyper-V*  
- **Windows 10 Home:** instalar *WSL2*  
- **Instalar Docker Desktop:** desde [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)

> Si da errores, borrar completamente Docker Desktop y reinstalar.

---

###  Instalación en MacOS
Descargar el paquete `.dmg` desde [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) y seguir los pasos.

---

### Playgrounds
Puedes usar **Play with Docker** para probar Docker sin instalar nada:  
 [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)

---


---

##  UD 03.01 – Principales acciones con Docker

###  Introducción
Comandos básicos para gestionar contenedores Docker mediante la **línea de comandos** (CLI), sin interfaz gráfica.

---

###  Conceptos clave
#### Imágenes
- Plantillas de solo lectura para crear contenedores.
- Se componen de **capas**.
#### Contenedores
- Instancias ejecutables de las imágenes.
- Pueden iniciarse, detenerse y eliminarse.

---

###  Almacenamiento
- **Imágenes:** `/var/lib/docker/overlay2`
- **Contenedores:** `/var/lib/docker/containers`
- **Volúmenes:** para datos persistentes.

---

###  Registro: Docker Hub
Plataforma donde se almacenan imágenes (públicas o privadas).  
 [https://hub.docker.com](https://hub.docker.com)

---

###  Comandos principales

| Comando | Descripción |
|----------|--------------|
| `docker run` | Crea y arranca un contenedor |
| `docker create` | Crea un contenedor sin arrancarlo |
| `docker ps` | Lista contenedores activos (`-a` muestra todos) |
| `docker start/stop/restart` | Arranca, detiene o reinicia contenedores |
| `docker inspect` | Muestra información detallada |
| `docker exec` | Ejecuta comandos dentro del contenedor |
| `docker cp` | Copia archivos entre host y contenedor |
| `docker attach` | Conecta la terminal al contenedor |
| `docker logs` | Muestra los registros del contenedor |
| `docker rename` | Cambia el nombre del contenedor |

---

###  Parámetros importantes de `docker run`

| Parámetro | Función |
|------------|----------|
| `-i` | Modo interactivo |
| `-t` | Asigna terminal |
| `--name` | Asigna nombre al contenedor |
| `--rm` | Borra el contenedor al detenerse |
| `-d` | Ejecuta en segundo plano |
| `-p` | Mapea puertos (ej. `-p 1200:80`) |
| `-e` | Define variables de entorno |

---

###  Ejemplos útiles

**1. Acceder a Ubuntu con terminal**
```bash
docker run -it --name=miUbuntu ubuntu /bin/bash
```

**2. Servidor web Nginx**
```bash
docker run -d -p 1200:80 nginx
```

**3. Variable de entorno**
```bash
docker run -it -e MENSAJE=HOLA ubuntu bash
echo $MENSAJE
```


##  UD 04.01 - Gestión de imágenes en Docker

###  Listado y búsqueda de imágenes

```bash
docker images                # Lista imágenes locales
docker search ubuntu          # Busca imágenes en Docker Hub
docker images -f=reference="u*:*04"   # Filtros avanzados
```

###  Descarga y eliminación

```bash
docker pull alpine:3.10       # Descarga una imagen
docker history nginx          # Muestra historial de una imagen
docker rmi ubuntu:14.04       # Elimina una imagen
docker system prune -a        # Limpieza total
```

###  Creación de imágenes

Crear una imagen desde un contenedor:

```bash
docker commit -a "autor" -m "comentario" idContenedor usuario/imagen:version
```

Añadir una etiqueta a la imagen:

```bash
docker tag usuario/imagen:version usuario/imagen:latest
```

###  Exportar e importar imágenes

```bash
docker save -o backup.tar imagen
docker load -i backup.tar
```

###  Subir imágenes a Docker Hub

```bash
docker login
docker push usuario/imagen
```

###  Crear imágenes con Dockerfile

**Ejemplo básico:**
```dockerfile
FROM ubuntu:latest
RUN apt update && apt install -y nano
CMD /bin/bash
```

**Construcción:**
```bash
docker build -t ubuntunano ./
```

**Comandos importantes de Dockerfile:**
| Comando | Descripción |
|----------|--------------|
| `FROM` | Define la imagen base |
| `RUN` | Ejecuta comandos durante la construcción |
| `CMD` | Define el comando por defecto |
| `EXPOSE` | Expone puertos internos |
| `COPY` / `ADD` | Copia archivos al contenedor |
| `ENTRYPOINT` | Define el proceso principal |
| `USER` | Usuario por defecto |
| `WORKDIR` | Directorio de trabajo |
| `ENV` | Variables de entorno |
| `ARG`, `VOLUME`, `LABEL`, `HEALTHCHECK` | Parámetros avanzados |

###  Optimización de imágenes

- Usar imágenes base ligeras (`alpine`, `scratch`).  
- Evitar paquetes innecesarios.  
- Unificar comandos `RUN`.  
- Limpiar caché y temporales:
  ```bash
  rm -rf /var/lib/apt/lists/*
  apt-get clean
  ```

---
##  UD 05.01 - Docker CheatSheet

###  Gestión de redes

```bash
docker network create redtest
docker network ls
docker network rm redtest
docker run -it --network redtest ubuntu /bin/bash
docker network connect IDRED IDCONTENEDOR
docker network disconnect IDRED IDCONTENEDOR
```

### Gestión de volúmenes

```bash
# Volumen ligado al host
docker run -d -it -v /home/sergi/target:/app nginx:latest

# Volumen Docker
docker run -d -it -v micontenedor:/app nginx:latest

# Crear / listar / eliminar volúmenes
docker volume create
docker volume ls
docker volume rm mivolumen

# Volumen temporal
docker run -d -it --tmpfs /app nginx

# Copia de seguridad de volúmenes
docker run --rm --volumes-from cont1 -v /home/sergi/backup:/backup ubuntu bash -c \
"cd /datos && tar cvf /backup/copia.tar ."

# Borrar todos los volúmenes
docker volume rm $(docker volume ls -q)
```

---
