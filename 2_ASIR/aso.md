<h1 style="text-align:center;font-size:40px;">ASO</h1>

## DOCKER

```bash
docker run --name webserver01 -d -p 8080:80 httpd
docker run --name webserver02 -v /origen:/destino -d -p 8081:80 httpd
docker exec -ti webserver02 /bin/bash # ejecutrar terminal en el contenedor
docker tag debian-ssh:latest debian-ssh:1 # cambiar etiqueta imaxe
echo "root:$ROOT_PASSWORD" | chpasswd # cambiar contraseña (para script .sh)
docker build -t debian-ssh:1 . ⇔ /*/debian/dockerfile # construir la imagen
docker run --name pass -d -p 2200:22 -e ROOT_PASSWORD=abc123. debian-ssh:1
```

### DOCKER MACHINE

```bash
docker-machine create -d virtualbox hostX
docker-machine ssh hostX "docker swarm init --advertise-addr x.x.x.x"
docker swarm join ..token # iniciamos swarm ⇔ saca un token
docker-machine ssh host02 "docker swarm join..." # unimos la otra máquina
docker-machine env host01 # desplegar aplicación
eval $(docker-machine env host01) # ejecutamos en el host el commando
docker stack deploy --compose-file docker-compose.yml wordpress
```

## BIND

## LDAP

```bash
apt-get install slapd
ldapsearch -x -b 'dc=breogan,dc=local' # buscar
ldapadd -D cn=admin,dc=breogan,dc=local -W -f X.ldif # añadir archivo
```

### CONFIGURACION PARA AÑADIR A LA MAQUINA

```bash
apt-get install libpam-ldapd
```

### SCRIPTS

```bash
apt-get install ldapscripts
ldapadd...
```

![](https://i.imgur.com/mdu1mzH.png)