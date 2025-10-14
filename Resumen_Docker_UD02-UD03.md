# 🐳 Docker - Instalación y Principales Acciones

## 📘 UD 02.01 – Instalación de Docker

### 🧩 Introducción
Guía para instalar Docker en diferentes sistemas operativos. Se recomienda **usar Linux (Ubuntu)** por su estabilidad y mejor soporte.

---

### ⚙️ Instalación en Linux (Ubuntu)
#### 🔸 Instalación recomendada (desde repositorio oficial de Docker CE)
**Pasos:**
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

### 🔧 Postinstalación
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

### ❌ Desinstalar Docker
```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker system prune -a
sudo rm -rf /var/lib/docker
```

---

### 💻 Instalación en Windows
- **Windows 10 Pro/Server:** activar *Hyper-V*  
- **Windows 10 Home:** instalar *WSL2*  
- **Instalar Docker Desktop:** desde [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)

> Si da errores, borrar completamente Docker Desktop y reinstalar.

---

### 🍏 Instalación en MacOS
Descargar el paquete `.dmg` desde [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac/) y seguir los pasos.

---

### 🌐 Playgrounds
Puedes usar **Play with Docker** para probar Docker sin instalar nada:  
👉 [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)

---

### 🧭 Conclusión
Usar **Linux (Ubuntu)** siempre que sea posible para minimizar errores y aprovechar la robustez del sistema.

---

## 📗 UD 03.01 – Principales acciones con Docker

### 🚀 Introducción
Comandos básicos para gestionar contenedores Docker mediante la **línea de comandos** (CLI), sin interfaz gráfica.

---

### 🧱 Conceptos clave
#### Imágenes
- Plantillas de solo lectura para crear contenedores.
- Se componen de **capas**.
#### Contenedores
- Instancias ejecutables de las imágenes.
- Pueden iniciarse, detenerse y eliminarse.

---

### 💾 Almacenamiento
- **Imágenes:** `/var/lib/docker/overlay2`
- **Contenedores:** `/var/lib/docker/containers`
- **Volúmenes:** para datos persistentes.

---

### 🐙 Registro: Docker Hub
Plataforma donde se almacenan imágenes (públicas o privadas).  
🔗 [https://hub.docker.com](https://hub.docker.com)

---

### 🔧 Comandos principales

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

### 🔠 Parámetros importantes de `docker run`

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

### 🧪 Ejemplos útiles

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

---

### 🧭 Conclusión
Con estos comandos y parámetros podrás **crear, gestionar y entender el funcionamiento básico de Docker**.

---

**📚 Fuente:** [Docker Docs](https://docs.docker.com/)
