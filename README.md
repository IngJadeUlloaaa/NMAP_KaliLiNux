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
