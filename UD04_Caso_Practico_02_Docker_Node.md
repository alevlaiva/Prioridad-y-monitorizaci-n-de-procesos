# UD04 – Caso Práctico 2
## Crear una imagen Docker con una app de ejemplo en Node.js

## 1. Introducción

En este caso práctico vamos a crear una **imagen Docker** que incluya una **aplicación de ejemplo en Node.js**. El ejercicio está basado en la documentación oficial de Docker:

- https://docs.docker.com/get-started/02_our_app/

La aplicación de ejemplo se obtiene del repositorio oficial:

- https://github.com/docker/getting-started/tree/master/app

---

## 2. Descargando la aplicación

1. Creaamos un directorio de trabajo (por ejemplo `Caso4-2`).
2. Descargamos la aplicación desde el repositorio oficial.
3. Dentro del directorio deben existir:
   - `package.json`
   - `yarn.lock`
   - Directorio `src/`
   - Directorio `spec/`





## 3. Preparando el Dockerfile

En el directorio del proyecto creamos el archivo `Dockerfile` con el siguiente contenido:

```Dockerfile
# Imagen base
FROM node:12-alpine

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiamos todo el contenido del proyecto
COPY . .

# Instalamos dependencias necesarias en producción
RUN yarn install --production

# Comando de arranque del contenedor
CMD ["node", "src/index.js"]
```

### Explicación 
- `FROM`: usa Node.js ligero basado en Alpine
- `WORKDIR`: crea y usa `/app` como directorio de trabajo
- `COPY`: copia el proyecto al contenedor
- `RUN`: instala dependencias
- `CMD`: arranca la aplicación

---

## 4. Creando la imagen Docker

Desde el directorio donde está el `Dockerfile`, ejecutamos:

```bash
docker build -t sampledocker ./
```

Esto crea una imagen llamada `sampledocker` usando el `Dockerfile` del directorio actual.

---

## 5. Probando la imagen

Para ejecutar el contenedor y exponer el puerto 3000:

```bash
docker run -dp 3000:3000 sampledocker
```

Después, abre el navegador y accede a:

```
http://localhost:3000
```
<img width="1512" height="725" alt="image" src="https://github.com/user-attachments/assets/bfbfcf26-8055-4d4a-8823-1a847a421266" />


---
