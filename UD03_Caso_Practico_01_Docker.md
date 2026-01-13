# Introducción a Docker  
## UD 03 – Caso práctico 01  
### Práctica de comandos en contenedor Docker



## 1. Preparando el contenedor

Crearemos un contenedor con la imagen base **ubuntu** y dejaremos lista una shell:

```bash
docker run -it --name ejercicio ubuntu /bin/bash
```
<img width="1231" height="124" alt="image" src="https://github.com/user-attachments/assets/a9fa8433-19bd-497e-9160-1b99795f6c52" />

Para salir del contenedor:

```bash
exit
```

Para volver a entrar:

```bash
docker start -ai ejercicio
```
<img width="1231" height="48" alt="Screenshot 2026-01-13 at 08 26 14" src="https://github.com/user-attachments/assets/4d693dd2-2e09-4a27-b680-6af2740c2f20" />

---

## 2. Solucionando el ejercicio

La solución consiste en crear 10 carpetas (del 01 al 10).

### Solución en Shell Script

```bash
#!/bin/bash

for i in {1..10}
do
  if [ $i -lt 10 ]
  then
    mkdir 0$i
  else
    mkdir $i
  fi
done
```

### Solución en Python 3

```python
#!/usr/bin/python3
import os

for x in range(1, 11):
    if x < 10:
        os.mkdir("0" + str(x))
    else:
        os.mkdir(str(x))
```
> Nota: instalar Python si no está disponible  
```bash
apt install python3
```

---

## 3. Script para comprobar la práctica

```bash
#!/bin/bash

for i in {1..10}
do
  if [ $i -lt 10 ]
  then
    docker exec -it $1 test -d /root/0$i
  else
    docker exec -it $1 test -d /root/$i
  fi

  if [ $? -ne 0 ]
  then
    echo "PRÁCTICA INCORRECTA"
    echo "ERROR EN PRUEBA ${i}"
    exit
  fi
done

echo "PRÁCTICA OK"
```


