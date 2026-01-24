# Introducción a Docker  
## UD 04 – Caso Práctico 01  
### Creando imagen Ubuntu con nano




## Índice

1. Introducción  
2. Preparando el Dockerfile y creando la imagen  
3. Probando la imagen  

---

## 1. Introducción

En este caso práctico vamos a crear y probar una imagen basada en **Ubuntu** que incluirá  
simplemente el editor de texto de consola **nano**.

---

## 2. Preparando el Dockerfile y creando la imagen

Crearemos el siguiente archivo `Dockerfile`:

```dockerfile

FROM ubuntu

# Actualizamos lista de paquetes e instalamos nano (-y para no preguntar)
# Las últimas líneas son para hacer la imagen más ligera
RUN apt update && apt install -y nano && apt purge --auto-remove && apt clean && rm -rf /var/lib/apt/lists/*

# Establecemos como comando por defecto de la imagen /bin/bash
CMD /bin/bash
```
![alt text](image-4.png)
El funcionamiento del `Dockerfile` está definido por sus propios comentarios.

Una vez preparado, crearemos la imagen con el siguiente comando:

```bash
docker build -t ubuntunano ./
```

Con esta línea indicamos que creamos la imagen **ubuntunano** basándonos en el fichero  
`Dockerfile` del directorio actual.

---

## 3. Probando la imagen

Con el siguiente comando podremos crear un contenedor con esta imagen, acceder a una shell  
dentro del contenedor y comprobar que el programa **nano** está instalado:

```bash
docker run -it ubuntunano
```

Una vez dentro del contenedor, podemos probar:

```bash
nano prueba.txt
```

---

