<h1 style="text-align:center;font-size:40px;">PAR</h1>

## CISCO

### VLAN

```cisco
(config)#VLAN X
(config-if)#name NAME
(config-if)#exit
(config)#interface VLANX
(config-if)#ip address <ip><mask>
(config-if)#no shutdown
(config-if)#exit
(config)#interface fa0/x ⇔ interface range fa0/x-y
(config-if)#switchport access vlan X
```

### Crear subinterfaces

```cisco
(config)#interface gig0/0.10
(config-subif)#encapsulation dot1Q 10
(config-subif)#ip address <ip><mask>

(config)#interface gig0/x
S1(config-if)#switchport mode trunk
```

### contraseñas:

```cisco
(config)#enable password cisco
(config)#enable password cisco2

(config-line)#password cisco
(config-line)#login
(config)#line vty 0 15
(config-line)#password cisco
(config-line)#login
(config)#service password-encryption
```

### SSH

```cisco
Telnet <ip> ## entrar en el router desde PC
#show running-config # comprobar contraseñas
(config)#service password-encryption # cifrar contraseñas
```

### Cifrar comunicaciones

```cisco
(config)#ip domain-name netacad.pkt # Conf nombre dominio
(config)#crypto key generate rsa # Genera claves seguras
(config)#username administrator secret cisco # Crear usuario para SSH
(config)#line vty 0 15 # Conf VTY admita solo SSH
(config-line)#transport input ssh
(config-line)#login local
(config-line)#no password cisco
ssh -l administrator <ip>
```

### ACL

> Declarar reglas,se van poniendo de 10 en 10 por si se añaden más.

```
10 deny 192.168.10.0 0.0.0.255
20 permit any
```

> - permitir tráfico de un pc con la ip 192.168.10.12, red 192.168.10.0,
> - añadir una regla antes de la 10 donde indicamos la ip y la máscara al
>   especificar un host concreto queda con todo ceros (0.0.0.0) quedando así:

```
9 permit 192.168.10.12 0.0.0.0
10 deny 192.168.10.0 0.0.0.255
20 permit any
R1(config)#no access-list 1
R1(config)#access-list 1 deny host <ip>
R1(config)#access-list 1 permit <ip> <mask (0.0.0.255)>
R1(config)#interface s0/0/0
R1(config)#ip access-group 1 out
```

![](https://i.imgur.com/0dEJfrV.png)
![](https://i.imgur.com/W3xBLzE.png)
