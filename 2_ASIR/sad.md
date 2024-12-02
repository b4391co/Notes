<h1 style="text-align:center;font-size:40px;">SAD</h1>

## Crear puente

```bash
brctl addbr LAN # crear un puente LAN
ip link set LAN up # activar
ip a s # ver
ip addr del 172.20.3.3/16 dev eth0 # eliminar la ip de eth0 (se pierde la conexión)
ip addr add 172.20.3.3/16 dev LAN # añadir la ip a LAN
brctl addif LAN eth0 # añadir la interfaz eth0 a LAN
brctl show
ip route show
ip route add default via 172.20.0.1 # volver a añadir la ip y recuperar la conexión /proc/sys/net/ipv4/ip_forward
```

### Crear puente en el arranque

![](https://i.imgur.com/Zi93Lhk.png)

![](https://i.imgur.com/SnRenTz.png)

### MV + SCRIPT

```bash
Qemu-img create -fqcow2 -b /var/lib/libvirt/templates/console -f row
encrypt-fs
```

Crear debian en maquina > conectar por ssh

```bash
ssh key-gen # Generar pareja de claves publicas y privadas
~/.ssh/authorized_keys # Crear carpeta en ~ Root
```

```bash
sed -i "s/viejo/nuevo/g" ruta # sustituir
ip route ver # tabla de rutas (gateway)
ip route | grep default | cut -d ' ' -f3 # saca gateway con filtros (se pueden poner en variables)
```

```bash
apt install encfs
mkdir privado y .privado # 2 carpetas
encfs .privado privado # (ruta completa)
```

```bash
apt install pam_mount
vim /etc/security/pam_mount.conf.xml # configurar montaje
```

```bash
<volume user="usuario" pstype="fuse" path="encfs#/home/%(USER)/.ofye" mountpoint="/home/%(USER)/pd" options="nonempaty" />
```

## conf network

```bash
ip addr del 172.20.200.174/16 dev enp1s0
ip route show
ip route del defaults
ip route add default via 192.168.122.1
```

## cryptsetup

### abrir unidad

```bash
cryptsetup luksFormat /dev/X # Crea cabeceira LUKS cos datos sobre o cifrado
cryptsetup open /dev/X Nombre
```

## abrir ssh

```bash
iptables -t nat -a PREROUTING -p tcp --dport 23 -j DNAT --to-destination DestIP:22
ssh -L 8080:www.google.es:80 user@ip -p23 # tunel directo
ssh - 8080:192.168.3.1:22 user@ip -p23 # tunel in
```