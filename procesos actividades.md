# actividades procesos
### Comunicación de procesos: señales de teclado y valores de retorno
1. **CTRL+C detener procesos**
```bash
 echo $?
 130
   ```
2. **CTRL+Z detener procesos**
```bash
[1]  + 39053 suspended  yes
```
---
### Comunicación de procesos con el orden kill y captura de señales

1. **Crear script trap.sh**
```bash
#!/bin/bash
#trap.sh Script para capturar señales
 
N = 0
while true
don
( ( N++ ) )
if [ $N -gt 200000 ] ; then
exit 1
fin
done
exit 0
```
2. **Darle permisos de ejecucion**
```bash
chmod +X trap.sh
```
#### He tenido problemas con el script

---
### Directorio /etc/proc
1. **Obtener pid del navegador**
```bash
 ps -l p 136450
 ```
 ```bash

 F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY        TIME CMD
1 S  1000  136450    2474  0  80   0 - 369210058 futex_ tty2   0:00 /usr/lib64/chromium-browser/chromium-browser
```
```bash
ls -m /proc/PID proceso
```
#### podemos ver toda la informacion del proceso