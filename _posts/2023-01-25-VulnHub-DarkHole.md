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

` Aplicamos un ataque con gobuster utilizando el diccionario big.txt a la ip de el servicio HTTP `

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


