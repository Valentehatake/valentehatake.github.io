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

# [](#header-1)2).Descubrimiento y enumeracion de puertos con nmap... 

`Realizamos un escaneo sencillo a la ip con nmap`

```js
#sudo nmap -n 10.10.60.74 -oG allports
```

![imagen](/images/Bounty_Hacker/nmapallports.png)

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





