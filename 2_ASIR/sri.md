<h1 style="text-align:center;font-size:40px;">SRI</h1>

www.microsoft.com. . = root hints

. |  | . | .com | | . | www | |
---------|----------|---------|---------|---------|---------|---------|---------|
.com | IP | > | microsoft.com | IP | > | www.microsoft.com | 10.25.45.50
.net | IP | . | telefonica.com | IP | . |  | 
.es  | IP | . |  |  | . |  |

## DNS

```bash
/etc/network/interfaces # cambiar ip en cliente y Router
/etc/resolv.conf 8.8.8.8 # cambiar DNS para instalar
Apt install dnsutils ⇔ Apt install tshark
Admin servidor Administrar agregar roles/características roles servidor DNS
liente cambiar dns
nslookup [direccion] # devolve dirección IP
dig@[IP] # devolve o nome
```

### LINUX

```bash
/etc/network/interfaces /etc/resolv.conf # cambiar ip e DNS
/etc/hosts /etc/hostname # cambiar nome máquina
apt install bind9
apt install tshark dnsutils # instalar utilidades
/etc/bind # ficheros de configuración BIND9
/etc/bind/named.conf.local # crear zonas locales de resolución, 2 archivos incluyen las entradas para zona de resolución directa e inversa (hay que crearlos)

zone "example.com" {
    type master;
    file "db.example.com";
};
zone "X.168.192.in-addr.arpa" {
    type master;
    file "db.192.168.X";
};

/var/cache/bind # Crear archivos db.example.com db.192.168.X
-rw-rw---- 1 bind bind 313 mar 14 16:25 db.192.168.X # Cambiar permisos
```

### Directa

```bash
vim /var/cache/bind/db.example.com // directa
```

```bash
$ORIGIN 2.168.192.in-addr.arpa.
$TTL 86400 ; 1 day
    @ IN SOA avatar hostmaster(
    1 ; serial
    21600 ; refresh
    3600 ; retry (1 week)
    604800 ; expire
    21600 ; minimum (6 hours)
)
@ IN NS avatar.example.com.
1 IN PTR avatar.example.com.
```

![](https://i.imgur.com/YuHejNP.png)

### Inversa

```bash
vim /var/cache/bind/db.192.168.X // inversa
```

```bash
$ORIGIN 2.168.192.in-addr.arpa.
$TTL 86400 ; 1 day
@ IN SOA avatar hostmaster(
    1 ; serial
    21600 ; refresh
    3600 ; retry (1 week)
    604800 ; expire
    21600 ; minimum (6 hours)
)
@ IN NS avatar.example.com.
1 IN PTR avatar.example.com.
```

```bash
/etc/init.d/bind9 restart ⇔ systemctl restart bind9
```

![](https://i.imgur.com/RQL7WBX.png)

### FORWARDERS

```bash
vim /etc/bind/named.conf.local
```

```bash
zone "other.sri"{
type forward;
forwarders {192.168.X.X}; // DNS
};
```

![](https://i.imgur.com/6T83AeN.png)

```bash
vim /etc/bind/named.conf.options
```

```bash
options {
    directory "/var/cache/bind";
    dnssec-validation no;
    dnssec-enable no;
    forward only;
};
```

![](https://i.imgur.com/pBrFlSO.png)

### MAESTRO ESCLAVO

```bash
vim /etc/bind/named.conf.options
```

```bash
options{
    allow-transfer {none;};
    ...
```

```bash
vim /etc/bind/named.conf.local
zone "X" {
    ...
    allow-transfer{x.x.x.x;}; ← IP DNS secundario
    notify yes;
```

```bash
vim /var/cache/bind/db.x.x.x
```

```bash
...                         ...
@   IN  NS  ns2.name.sri.   =
```

```bash
vim /var/cache/bind/db.name.sri
```

```bash
...                             ...
ns2 IN  A   x.x.x.x (IP DNS 2)  5   IN  PTR     ns2.*
```

```bash
vim /etc/bind/named.conf.local
```

```bash
zone "X" {
    type eslave;
    file "db.name.sri"; 
    masters {x.x.x.x;}; (IP DNS 1)
}
```

### SUBDOMINIO VIRTUAL

```bash
vim /var/cache/bind/db.nome.sri # modificar para añadir ns3
```

```bash
...
$ORIGIN es.example.sri.
@   IN  NS  ns3
ns3 IN  A   x.x.x.x
```

![](https://i.imgur.com/kmypDwc.png)

```bash
vim /etc/bind/noamed.conf.local # añadir el subdominio
```

```bash
zone "es.example.sri" {
    type master;
    file "db.es.example.sri";
};
```

```bash
vim /var/cache/bind/db.es.nome.sri # crear
```

```bash
masters {x.x.x.x};
    file "informatica.example.com";
};
```

![](https://i.imgur.com/RLxsRmP.png)

![](https://i.imgur.com/4a0L27c.png)

### STUB

```bash
zone "informatica.example.com" {
    type stub;
    masters {x.x.x.x};
    file "informatica.example.com";
};
```

![](https://i.imgur.com/sm22Pdd.png)

### VISTAS

`/etc/bind/named.conf.local`

#### crar ACL

##### vista interna

zone: name.sri

&nbsp;&nbsp;&nbsp;&nbsp; db.int.name.sri

zone: x.x.x.in... .sri

&nbsp;&nbsp;&nbsp;&nbsp; db.int.x.x.x

##### vista externa

zone: name.sri

&nbsp;&nbsp;&nbsp;&nbsp; db.ext.name.sri

zone y.y.y.in... .sri

&nbsp;&nbsp;&nbsp;&nbsp; db.ext.y.y.y

`/var/cache/bind/db.int.name.sri`

![](https://i.imgur.com/h2Th47p.png)

`/var/cache/bind/db.int.x.x.x `

![](https://i.imgur.com/eOzgOmh.png)

`/var/cache/bind/db.ext.name.sri`

![](https://i.imgur.com/xKQYXCw.png)

`/var/cache/bind/db.ext.y.y.y`

![](https://i.imgur.com/Bo0n04g.png)

### DHCP

![](https://i.imgur.com/yQk1G0x.png)

```bash
apt-get install isc-dhcp-server
vim /etc/default/isc-dhcp-server // archivo conf
```

### SUBNET

```bash
/etc/defaults/isc-dhcp-server
```

ASIGNAR IP FIJA MEDIANTE MAC

![](https://i.imgur.com/2hPXTq8.png)

![](https://i.imgur.com/lVrSp3h.png)

### ACTIVAR ROUTER FORWARD

```bash
/etc/network/interfaces
```

![](https://i.imgur.com/nRcIFxH.png)

### DDNS

#### DNS

`/etc/bind/named.conf`

![](https://i.imgur.com/qDzlvxY.png)

`/etc/bind/named.conf.local`

![](https://i.imgur.com/jfqAl29.png)

#### DHCP

`/etc/dhcp/dhcpd.conf`

![](https://i.imgur.com/jgTBdsP.png)

![](https://i.imgur.com/xQfH5n3.png)

### SERVIDOR WEB

#### APACHE

##### CONFIGURAR DNS

![](https://i.imgur.com/ZoypzrU.png)

![](https://i.imgur.com/MGmmBJ1.png)

![](https://i.imgur.com/rQxW8R4.png)

![](https://i.imgur.com/QbGFOlh.png)

##### CONF APACHE

```bash
vim /etc/apache2/sites-available 
cp 000-default.conf new.conf // crear archivo conf
chmod a+x new.conf // chmod a-x 000...conf 
a2ensite new // crear ligacion simbolica 
mkdir /var/www/new
chown -R www-data:www-data /var/www/new
```

![](https://i.imgur.com/chMopez.png)