# NMAP_KaliLiNux

1. 🔌 ¿Qué es una red?
Una red informática conecta dispositivos para compartir información. Por ejemplo, tu computadora conectada por Wi-Fi a un router, y este a internet.

2. 🌐 Conceptos clave de redes
🧭 IP (Internet Protocol)
Es una dirección única que identifica un dispositivo en una red.

Ejemplo: 192.168.1.10

Tipo:

IPv4: 192.168.0.1 (el más común)

IPv6: fe80::a4b3:cc22::... (más largo)

📦 TCP y UDP
Protocolo	Tipo	Características principales
TCP	Conexión	Fiable, verifica si los datos llegaron. Ej: HTTP, FTP
UDP	Sin conexión	Rápido, pero no garantiza entrega. Ej: DNS, streaming

🔢 Puertos
Cada servicio usa un puerto para comunicarse.

Ejemplo:

80: HTTP

443: HTTPS

21: FTP

22: SSH

3306: MySQL

En pentesting, usamos herramientas como nmap para descubrir puertos abiertos.

📛 DNS (Domain Name System)
Traduce dominios a IPs.

Ej: facebook.com → 157.240.1.35

🔄 ARP (Address Resolution Protocol)
Traduce IP a MAC address (identificador físico de un dispositivo).

Puedes usar arp -a en Windows o ip neigh en Linux para ver quién está en tu red.

3. 🛠️ ¿Qué es Nmap y cómo se usa?
✅ ¿Qué es Nmap?
Herramienta de escaneo de red.

Permite descubrir:

Dispositivos conectados

Puertos abiertos

Servicios

Sistemas operativos

✅ Instalación (ya viene con Kali Linux):
bash
Copy
Edit
sudo apt update
sudo apt install nmap
📌 Comandos clave de Nmap
bash
Copy
Edit
# Escanear una IP con puertos comunes
nmap 192.168.1.10

# Escaneo de red completa
nmap 192.168.1.0/24

# Detectar sistema operativo (OS fingerprinting)
sudo nmap -O 192.168.1.10

# Escaneo agresivo (reconocimiento profundo)
sudo nmap -A 192.168.1.10

# Solo escaneo de puertos específicos
nmap -p 21,22,80,443 192.168.1.10
🔍 Resultado típico:
arduino
Copy
Edit
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
Eso te dice que el objetivo tiene el puerto 80 y 443 abiertos → posible servidor web.
------------------------------------------------------------------------------------------------------------------------------------------------------
Cómo interpretar la salida de ip a
Cuando ejecutás:

bash
Copy
Edit
ip a
Vas a ver varias secciones que corresponden a interfaces de red de tu equipo, por ejemplo:

sql
Copy
Edit
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 11:22:33:44:55:66 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.25/24 brd 192.168.1.255 scope global dynamic wlan0
       valid_lft 86334sec preferred_lft 86334sec
    inet6 fe80::aabb:ccdd:eeff:1122/64 scope link 
       valid_lft forever preferred_lft forever
1. ¿Qué es cada sección?
wlan0: interfaz Wi-Fi (si estás conectado por Wi-Fi)

eth0 o enp0s3: interfaz Ethernet (cableada)

lo: interfaz local (loopback), que siempre tiene IP 127.0.0.1, no es la que usas para conectarte a la red.

2. ¿Cuál es mi IP?
Busca la línea que empieza con inet (sin el 6):

nginx
Copy
Edit
inet 192.168.1.25/24
Esta es tu dirección IPv4 local.

El /24 indica la máscara de red (máscara de subred), que significa que la red abarca las direcciones de 192.168.1.0 a 192.168.1.255.

Esa IP (192.168.1.25) es la que tu equipo tiene en la red local.

3. ¿Y el inet6 qué es?
Esa es tu dirección IPv6.

Es otro protocolo de red, más nuevo, para soportar más direcciones.

Puedes ignorarlo por ahora si solo quieres trabajar con IPv4.

4. Ejemplo completo
Supongamos esta salida:

sql
Copy
Edit
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    inet 192.168.1.25/24 brd 192.168.1.255 scope global dynamic wlan0
    inet6 fe80::aabb:ccdd:eeff:1122/64 scope link
Tu IP local es: 192.168.1.25

Tu red es: 192.168.1.0/24 (esto es el rango donde están todos los dispositivos conectados a tu router)

El brd 192.168.1.255 es la dirección de broadcast (para enviar mensajes a todos).

5. ¿Cómo saber la IP pública?
Tu IP pública (la que ves en internet) NO está aquí, para eso podés usar:

bash
Copy
Edit
curl ifconfig.me
O visitar sitios como https://whatismyipaddress.com desde tu navegador.

----------------------------------------------------------------------------------------------------------------------------------

Sobre las interfaces que ves: lo y eth0
Interfaz	Qué es	IP típica
lo	Loopback (local, interna de tu PC)	Siempre 127.0.0.1 (local)
eth0	Ethernet (cable o a veces Wi-Fi)	IP asignada en tu red local

Cómo saber cuál es tu IP local
En la sección de lo:

bash
Copy
Edit
inet 127.0.0.1/8
Esta IP no es tu IP real en la red. Es una dirección interna para que tu sistema se comunique consigo mismo.

En la sección eth0 (o a veces wlan0 si usás Wi-Fi), buscá la línea que dice inet:

bash
Copy
Edit
inet 192.168.1.25/24
Esta es tu IP local en la red Wi-Fi o Ethernet.

¿Y la IP de tu router?
Normalmente, la IP de tu router es la puerta de enlace predeterminada (default gateway).

Para verla, podés ejecutar:

bash
Copy
Edit
ip route | grep default
Ejemplo de salida:

nginx
Copy
Edit
default via 192.168.1.1 dev eth0 proto dhcp metric 100
Ahí 192.168.1.1 es la IP del router (puerta de enlace).

Resumen rápido
Lo que ves	Qué significa
lo → 127.0.0.1	Interfaz interna (no red externa)
eth0 → inet X	Tu IP en la red local (ej. 192.168.1.25)
Default gateway	IP del router (ej. 192.168.1.1)
