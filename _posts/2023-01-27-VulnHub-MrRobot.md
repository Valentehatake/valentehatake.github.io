---
title: VulnHub-Mr.Robot
---
<dt>WriteUp-CTF</dt>
<dd>Valente2023intuicion</dd>
# [](#header-1)Estructura del entorno de trabajo...  
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
 
```js
#sudo nmap -sV 192.168.1.85
   -Este comando realiza un escaneo de puertos en el dispositivo con dirección IP 192.168.1.85,
 utilizando la herramienta Nmap con la opción -sV, que indica que se realizará una detección de
 servicios y versiones con permisos elevados de root.
```
![imagen](/images/MrRobot/5.png)

- `Resultado `

| PORT         | STATE             |SERVICE|
|:-------------|:------------------|:------|
| 22/TCP       | Closed            | ssh   |
| 80/TCP       | Open              | http  |
| 443/TCP      | Open              |ssl/http|

- Servicio http desplegado.
![imagen](/images/MrRobot/6.png)

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


![imagen](/images/MrRobot/8.png)

![imagen](/images/MrRobot/9.png)

![imagen](/images/MrRobot/10.png)

![imagen](/images/MrRobot/11.png)

![imagen](/images/MrRobot/12.png)

![imagen](/images/MrRobot/13.png)

![imagen](/images/MrRobot/14.png)

![imagen](/images/MrRobot/15.png)

![imagen](/images/MrRobot/16.png)

![imagen](/images/MrRobot/17.png)

![imagen](/images/MrRobot/18.png)

![imagen](/images/MrRobot/19.png)

![imagen](/images/MrRobot/20.png)

![imagen](/images/MrRobot/21.png)

![imagen](/images/MrRobot/22.png)

![imagen](/images/MrRobot/23.png)

![imagen](/images/MrRobot/24.png)

![imagen](/images/MrRobot/25.png)

![imagen](/images/MrRobot/26.png)

![imagen](/images/MrRobot/27.png)

![imagen](/images/MrRobot/28.png)

![imagen](/images/MrRobot/29.png)

![imagen](/images/MrRobot/30.png)
