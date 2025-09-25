# Prioridad-y-monitorizacion-de-procesos
### ps aux 
La orden ps aux nos sirve para mostrar todos los procesos en ejecución
```
ps aux

```
```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.7  0.0 104512 13696 ?        Ss   09:02   0:05 /sbin/init splash
root           2  0.0  0.0      0     0 ?        S    09:02   0:00 [kthreadd]
```
### ps -l -p
Esta orden nos permite ver un proceso en concreto con el parametro -p y más información con el parámetro -l
```
ps -l -p 9039
```
```
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
1 S 1178032053 9039  8565  0  80   0 - 303516048 futex_ ?    00:00:00 chrome
```
### renice -n 30
esta orden nos permite cambiar la prioridad de un proceso
```
renice -n 30 9039
```
9039 (process ID) prioridad anterior 0, nueva prioridad 19

```
ps -l -p 9039
```
```
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
1 S 1178032053 9039  8565  0  99  19 - 303516048 futex_ ?    00:00:00 chrome
```
### top
me ha dado error
