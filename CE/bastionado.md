<h1 style="text-align:center;font-size:40px;">BASTIONADO</h1>

## DOCKER

```bash
apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common # Instalación de dependencias

curl -fsSL https://download.docker.com/linux/debian/gpg |apt-key add -
add-apt-repository "deb [arch=amd64] https:#download.docker.com/linux/debian $(lsb_release -cs) stable" # Añadir repositorio

apt update
apt install -y docker-ce docker-ce-cli containerd.io
usermod -aG docker $USER # Añadir usuario no root al grupo
newgrp docker

docker run -it --name webserver -h webserver nginx bash
⇔
docker pull nginx
docker create -name webserver nginx
docker run -it -name -h webserver webserver nginx bash

docker ps ⇔ docker ps -a
docker inspect webserver # Ver propiedades de un container	
docker run -d -p 80:80 --name webserver -h webserver nginx servicenginx
start # Ejecución del servicio web
docker rm -f webserver

docker run -d --name webserver nginx
docker exec -it webserver bash
```

### Dockerfile

```bash
vim Dockerfile
```

```bash
FROM debian:latest
ENV TERM xterm
LABEL maintainer=javierfp@iessanclemente.net
#Instalamos nginx
RUN apt update && apt install -y nginx
#Los container de la imagen escucharán en los puertos 80 y 443
EXPOSE 80 443
#Comando para arrancar nginx
CMD ["nginx","-g","daemon off;"]```p
```

> **FROM**: indica la imagen base sobre la que se construirá la actual. De este modo podemos crear imágenes nuevas a partir de otras.
> **LABEL**: permite añadir etiquetas descriptivas e información de la imagen
> **ENV**: permite definir variables de entorno
> **RUN**: permite ejecutar comandos durante la creación de la imagen resultante
> **EXPOSE**: expone los puertos indicados para que puedan ser accedidos en el container. No los mapea al host. Se accedería a los puertos indicados a través de la IP del container
> **CMD**: permite pasar un comando y sus argumentos, estableciendo el comando que se ejecutará cuando se cree un container a partir de esa imagen. Esta directiva es similar a la directiva ENTRYPOINT

```bash
docker build . -t nginximg
```

### Volúmenes locales

```bash
docker run -d -p 80:80 --name nginx -v nginxvol:/var/www nginx:latest
docker volume ls
docker volume inspect nginxvol

docker volume create # crear
docker volume rm # eliminar
docker volume prune # eliminar volúmenes no utilizados

docker inspect nginx

docker stop nginx && docker rm nginx
mkdir -p ~/www/miweb
docker run -d -p 80:80 --name nginx -v ~/www:/var/www nginx:latest
docker exec -it nginx bash

docker create -v websvol:/var/www --name webs nginx /bin/true
docker volume inspect webs
docker run -d --volumes-from webs --name nginx2 nginx
```

### Gestión de Network

#### Redes por defecto

```bash
docker network ls
```

> **La red host**: Conecta el container en la pila de red del host
> **La red none**: Mantiene al host aislado, sin red
> **La red bridge**: Es la que se utiliza por defecto cuando se lanzan los container sin especificar parámetros de red, Esta red permite que los containers conectados a ella puedan comunicarse a través de sus IPs.

```sh
docker network inspect bridge

docker network create --driver bridge minet

docker network inspect minet

docker run -it --name alpi1 --network=minet alpine
docker run -it --name alpi2 --network=minet alpine
docker exec -it alpi1 sh
docker attach alpi1 > ping alpi2
```

### Docker Compose

```sh
docker-compose up -d

docker-compose build
docker-compose create
docker-compose start
```

## GNS3

### Instalar ( Posibles errores )

```sh
sudo apt update
sudo apt install -y python3-pip python3-pyqt5 python3-pyqt5.qtsvg \
python3-pyqt5.qtwebsockets \
qemu qemu-kvm qemu-utils libvirt-clients libvirt-daemon-system virtinst \
wireshark xtightvncviewer apt-transport-https \
ca-certificates curl gnupg2 software-properties-common

apt install -y vpcs libvirt-clients libpcap-dev bridge-utils dynamips 
pip3 install gns3-server
pip3 install gns3-gui

sudo brctl addbr virbr0
sudo ip link set virbr0 up

git clone https://github.com/GNS3/ubridge
cd ubridge 
make
sudo make install
```

#### Terminal

```sh
wezterm start -- telnet %h %p
/usr/bin/vpcs
```

## SUBNETTING

**RL:** como puerta de enlace de SAT ADM DIR

**RDI:** como puerta de enlace de DMZ SAT

<div align='center'><img src='https://i.imgur.com/PXZQ1Gw.png'/></div>
 
![](https://i.imgur.com/DOYt9Gn.png)
![](https://i.imgur.com/PUFnB6l.png)
![](https://i.imgur.com/J1d8SLh.png)
![](https://i.imgur.com/mwZCCGz.png)


```sh
ADM1> ip addr add 10.10.10.1/25 dev eth0
SAT1> ip addr add 10.10.10.129/27 dev eth0
DIR1> ip addr add 10.10.10.161/27 dev eth0
DMZ1> ip addr add 10.10.10.193/27 dev eth0

RL> ip addr add 10.10.10.126/25 dev eth0 # ADM
RL> ip addr add 10.10.10.158/27 dev eth0 # SAT 
RL> ip addr add 10.10.10.190/27 dev eth0 # DIR

RDI> ip addr add 10.10.10.157/27 dev eth0 # SAT
RDI> ip addr add 10.10.10.222/27 dev eth0 # DMZ

ADM1> ip route add default via 10.10.10.126 # ADM utiliza como puerta de enlace RL
SAT1> ip route add 10.10.10.0/25 via 10.10.10.158 # SAT utiliza como puerta de enlace RL para comunicarse con ADM y DIR
SAT1> ip route add 10.10.10.160/27 via 10.10.10.158 # SAT utiliza como puerta de enlace RL
SAT1> ip route add 10.10.10.192/27 via 10.10.10.157 # SAT Utuliza como puerta de enlaze RDI para llegar a DMZ
DIR1> ip route add default via 10.10.10.190
DMZ1> ip route add default via 10.10.10.222

RL> ip route add default via 10.10.10.157
RDI> ip route add default via 10.10.10.158
```

## IPTABLES

```sh
# Listar
iptables -nL # tabla de filtros
iptables -L --line-numbers
iptables -t nat -L # tabla NAT

# Limpiar reglas
iptables -F 

# crear politica 
iptables -P INPUT ACCEPT # -P Politica , cadena INPUT , acción
iptables -P INPUT DROP # Politica por defecto en DROP - No pueden llegar paquetes de ninguna conexion

#Reglas - EJEMPLOS 
iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT # Paquetes solo de la red local - -A append ( añadir una regla ) , -j jump
iptables -A INPUT -p tcp --dport 80 -j ACCEPT # Solo trafico por un puerto
iptables -A INPUT -p tcp -dport 22 -j DROP # Bloquer ssh
iptables -A INPUT -p tcp -dport 2222:2230 -j DROP # Bloquer rango de puertos
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT # permitir trafico generado -  -m match , --state establecido, existe conexion
iptables -A OUTPUT -m state --state NEW,RELATED -j ACCEPT # Permite establecer la conexion para que funcione el anterior 
iptables -R OUTPUT 1 -m state --state NEW,ESTABLISHED -j ACCEPT  # Modificar ( -2 ) por error - 1 = Posicion ( --line-numbers )


# Eliminar una relga
iptables -D INPUT {RULE_NUMBER}

# Guardar
iptables-save > save.txt
iptables-restore < save.txt
```

### Reglas para hosts

```sh
# Políticas para todos los hosts
iptables -P OUTPUT DROP
iptables -P INPUT DROP

# Tráfico local
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Conexiones ESTABLISHED y RELATED para todos los hosts
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Tráfico de salida http/https y resolución DNS para todos los hosts
iptables -A OUTPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate NEW -j ACCEPT
iptables -A OUTPUT -p udp --dport 53 -d 8.8.8.8 -j ACCEPT
iptables -A OUTPUT -p udp --dport 53 -d 8.8.4.4 -j ACCEPT

# Tráfico entrante procedente de DNS para todos los hosts
iptables -A INPUT -p udp --sport 53 -s 8.8.8.8 -j ACCEPT
iptables -A INPUT -p udp --sport 53 -s 8.8.4.4 -j ACCEPT

# Acceso ssh desde sat para todos los hosts
iptables -A INPUT -p tcp --dport 22 -s 192.168.112.0/24 -m conntrack --ctstate NEW -j ACCEPT

# Tráfico entrante para host www
iptables -A INPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate NEW -j ACCEPT
```

### Reglas para puertas de enlace

```sh
# Políticas en todas las puertas
iptables -P OUTPUT DROP
iptables -P INPUT DROP
iptables -P FORWARD DROP
# Puerta RL
# Tráfico en ambas direcciones entre eth0 y eth1
iptables -A FORWARD -i eth0 -s 192.168.110.0/24 -o eth1 -d 192.168.111.0/24 -j ACCEPT
iptables -A FORWARD -i eth1 -s 192.168.111.0/24 -o eth0 -d 192.168.110.0/24 -j ACCEPT

# Tráfico en ambas direcciones entre eth0 y eth2
iptables -A FORWARD -i eth0 -s 192.168.110.0/24 -o eth2 -d 192.168.112.0/24 -j ACCEPT
iptables -A FORWARD -i eth2 -s 192.168.112.0/24 -o eth0 -d 192.168.110.0/24 -j ACCEPT

# Tráfico en ambas direcciones entre eth1 y eth2
iptables -A FORWARD -i eth1 -s 192.168.111.0/24 -o eth2 -d 192.168.112.0/24 -j ACCEPT
iptables -A FORWARD -i eth2 -s 192.168.112.0/24 -o eth1 -d 192.168.111.0/24 -j ACCEPT

# Tráfico ESTABLISHED y RELATED
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Tráfico NEW saliente por eth2 hacia http/https y DNS
iptables -A FORWARD -p tcp -o eth2 -m multiport --dports 80,443 -m conntrack --ctstate NEW -j ACCEPT
iptables -A FORWARD -p udp -o eth2 --dport 53 -d 8.8.8.8 -j ACCEPT
iptables -A FORWARD -p udp -o eth2 --dport 53 -d 8.8.4.4 -j ACCEPT

# Tráfico entrante de servicios DNS por eth2 en RL:
iptables -A FORWARD -p udp -i eth2 --sport 53 -s 8.8.8.8 -j ACCEPT
iptables -A FORWARD -p udp -i eth2 --sport 53 -s 8.8.4.4 -j ACCEPT
```