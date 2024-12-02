<h1 style="text-align:center;font-size:40px;">ISO</h1>

## Arranque

### Windows

#### Sistema

```bash
Compmgmt.msc # Administración de usuarios y grupos locales⤵
lusrmgr.msc # Para administrar rapidamente os usuarios
shell:startup / shell:common startup # Usuario / Todos
Devmgmt.msc # Administrador de Dispositivos
systeminfo # info hardware
sigverif # comprobar firma
```

#### CD

##### WINDOWS

```
SO ( WIN 7 )
> shift+f10

diskpart
list disk 
select disk 0 
list partition > clean 
( help create 
partition ( Primary ) 
create partition primary size=” ” 
List 
Partition -->select partition 1 
active
```

##### System Rescue CD

```
SO (Win XP)
CD : System Rescue Cd > 1 
startx
partition
boot xp // hidden Win7
V2C47-MK7JD-3R89F-D2KXW-VPK3J
```

#### Menu arranque Windows

##### XP

```
Disco local > Mostrar todo
boot.ini
MBR = Primer sector do disco duro
```

##### Win 7 +

```
Win 7
Cmd → system32
```

![](https://i.imgur.com/sX8SZPB.png)

### BCD

#### Regenerar BCD

```bash
Bcdboot C:\windows /s C: # Rexeneramos o arranque do Windows da 2º partición
Bootrec /Fixboot # Rexeneramos o Sector de arranque da partición
Bootrec /scanos # Rexeneramos o arquivo BCD
```

#### Clonar y restaurar

##### Clonar I ( copia de seguridad )

```bash
fdisk -l # Comprobar que o detecta o sistema
cfdisk /dev/sd(b) # Crear particiones desde cfdisk, de tipo 2
mkfs -t ext4 /dev/sdb1 # Formatear en ext4
mount /dev/sdb1 /mnt # Montar sdb1 en mnt
sfdisk -d /dev/sda /mnt/tp.bak # Facer copia de seguirdade da táboa de particións

dd if=/dev/sda of=/mnt/mbr.bak bs=512 count=1 // Copia 1º sector HD
```

##### Restaurar I

```bash
mount /dev/sdb1 /mnt # Montar o disco duro de imaxes en /mnt
Nano /mnt/boot.ini # editar boot.ini
sfdisk /dev/sda < /mnt/tp.bak # Restaurar copia de seguridade
```

##### Clonar II

```bash
mount | grep sd # filtra e amosa palabras que componen "sd"
umount /mnt
mount | grep sd
mount /dev/sdb1 /mnt
```

###### Linux

```bash
fsarchiver restfs /mnt/debianSistema.fsa id=0,dest=/dev/sdaX
mkswap /dev/sdaX
```

###### Windows

```bash
partclone.ntfs -c -s /dev/sda1 -o /mnt/win7.img
partclone.ntfs -c -s /dev/sda2 -o /mnt/winXP.img
```

##### Restauracion II

```bash
mount /dev/sdb1 /mnt
cfdisk /dev/sda
partclone.ntfs -r -s /mnt/winX.img -o /dev/sdaX
```

###### Win CD

```bash
bcdedit
bootrec /fixmbr
bootrec /rebuildbcd
```

###### Linux

```bash
fsarchiver restfs /mnt/debianSistema.fsa id=0,dest=/dev/sdaX
mkswap /dev/sdaX

mount /dev/sdaX /mnt
nano /mnt/etc/fstab
umount /mnt

> CD debian > advanced > modo rescate > Restaurar GRUB > 
grub.Install /dev/sda > update-gru
```

##### Clonar III

Configuracion inicio (p 11)
Facer imaxe
&nbsp;&nbsp; Intento 1 (p 13)
&nbsp;&nbsp;&nbsp;&nbsp; Sysprep
&nbsp;&nbsp;&nbsp;&nbsp; Restauro
&nbsp;&nbsp;&nbsp;&nbsp; Problema pregunta cambios
&nbsp;&nbsp; Intento 2 Sysprep arquivo respostas (p 17)
&nbsp;&nbsp;&nbsp;&nbsp; Fago sysprep respostas
&nbsp;&nbsp;&nbsp;&nbsp; Restauro
&nbsp;&nbsp;&nbsp;&nbsp; Problema :config dos usuarios
&nbsp;&nbsp; Sysprep con bat
&nbsp;&nbsp;&nbsp;&nbsp; creo BAT
&nbsp;&nbsp;&nbsp;&nbsp; sysprep config
&nbsp;&nbsp;&nbsp;&nbsp; Reinicio == Solucionado

## LINUX AVANZADO

### GRUB

```bash
/etc/grub.d # no modificar
/etc/default/grub
update-grub
etc/grub.d # archivos editar (editar 40_custom, quitar permisos 30_os-pr)
```

### APT

#### REPOS

```bash
/nvim /etc/apt/sources.list
```

```bash
#
Tipo paquete Url Repositorio Version Debian Sección
deb http://deb.debian.org/debian buster main
deb-src http://ftp.es.debian.org/debian testing main
#deb-src http://deb.debian.org/debian stable main
deb http://deb.debian.org/debian buster-backports main
deb http://deb.debian.org/debian-securirty buster/updates main
```

```bash
mkdir ~/src
cd ~/src
apt-get source "package"
apt-get install build-essential
apt-get -t buster-backports install "package
```

### INSTALAR SERVIDOR X

```bash
apt install xorg
xmayland # Servidor X
startx
```

#### GESTOR DE VENTANAS

```bash
apt-cache search window manager | grep "window manager"
apt install wmaker // icewm // e17 // Gestores de ventanas
apt install lxde // xfce4 // mate // gnome // Entorno de escritorio
apt install xdm // wdm // lightdm // Display Manager # gestor login
dpkg-reconfigure xdm # Cambiar Gestor Login
```

### USUARIOS

```bash
whoami
Passwd
cat/etc/Passwd
vim /etc/shadow

shadowconfig on ⇔ off
```

adduser ⇔ useradd
deluser
usermod

#### Group

```bash
groupmod -g gid nomegrupo # Cambiar GID
groupmod -n NuevoNombre NombreGrupo # Cambiar nombre
groupmod -G (Principal) -g (Secundario) NombreGrupo User
groups usuario # Grupos del usuario
```

##### GPASSWD

```bash
gpasswd -A usuario grupo # Convertir en admin
gpasswd -M " " # Quitar Admin
gpasswd grupo # Establecer contraseña grupo
gpasswd -r grupo # Quitar contraseña
gpasswd -a usuario group # Añadir miembros
gpasswd -d " " # Eliminar miembro
```

##### ACL SETFACL

```bash
setfacl -m group:grupo2:rwx /dir1 # Permisos Grupo
setfacl -m user:usuario:rwx /dir1 # Permisos User
setfacl -x user:usuario /dir1 # Elimina user
setfacl -R # Cambia permisos de forma recursiva
setfacl -b # Borra permisos conserva UGO
```

```
/etc/skel #ficheros/directorios que se copian en home de un nuevo usuario
```

### COMPRIMIR

#### GZIP

```bash
gzip test # Comprime archivo
gzip -l test.gz # Muestra ratio de compresión
gunzip test.gz ⇔ gzip -d test.gz # Descomprime
```

gzip solo comprime un archivo

#### TAR

TAR COMPRIME VARIOS A UNO -- GZIP COMPRIME UNO

```bash
tar [options(c/x/t/r/v/f)] archivos
```

#### JUNTAR TAR+GZ

```bash
tar czf test.tar.gz archivos ← Comprimir
tar xzf test.tar.gz ← Descomprimir
```

### COPIAS DE SEGURIDAD

```
rdiff-backup ORIGEN/ COPIA/ # Hacer copia de seguridad
rdiff-backup -l COPIA/ # Listar copias
rdiff-backup -r NombreCopia COPIA/ RESTAURADOS/ # Restaurar copia
```

### SYSTEMD

```bash
systemd-analyze (blame) # tiempo que tarda en arrancar (por servicio)
systemctl status Nombre # Info servicio
systemctl start/stop Nombre
```

`/lib/systemd/system/service.service` ← Ruta servicios

`journalctl` ← Logs sistema

### SIST. ARCHIVOS

#### PARTICIONAR

```bash
cfdisk
```

#### FORMATEAR

```bash
apt install mkfs-x
mkfs -t vfat -L DATOS /dev/sdx ⇔ mkfs.vfat # FAT32
mkfs -t ext4 -L NOMBRE /dev/sdx ⇔ mkfs.ext4 # EXT4
mkfs -t ntfs -f -L NOMBRE /dev/sdx ⇔ mkfs.ntfs # NTFS
```

##### MOUNT SOLO ROOT

```bash
nvim /etc/fstab

/dev/sda3 /mnt/win ntfs defaults 0 0

blkid # Conocer ID
```

#### NFS

```bash
apt install nfs-common # Cliente
apt install nfs-kernel-server # servidor
ss -4ta | grep nfs ⇔ ss -4ta | grep 2049 # Escuchar puertos
```

##### añadir carpetas

```bash
vim /etc/exports

/srv/compartida1 192.168.63.4(ro)
/srv/compartida2 192.168.0.0/255.255.0.0(rw)

systemctl restart nfs-kernel-server
mount -t nfs 192.168.63.100:/compartida /mnt/remota
```

#### Cuotas

```bash
vim /etc/fstab

/dev/sdax / ext4 defaults,errors=remount-ro,usrquota,grpquota 0 1

mount -o remount / # Remontar sin reboot
quotacheck -cugm / # Activar (si no reiniciamos)
quotaon / ⇔ quotaoff / # Habilitar ⇔ deshabilitar
repquota -a # Comprobar establecido en cuotas
```

##### Asignar cuotas

```bash
edquota -u user # usuarios
edquota -g group # Grupo
```

## Windows avanzado

### REGISTRO

> El Registro es una base de datos que contiene toda la información de
> configuración de Windows. En él se incluye absolutamente todo, desde las
> opciones de pantalla hasta las contraseñas de usuario.
> La información del Registro se organiza por categorías
> El Registro está organizado en una estructura jerárquica, compuesta por
> subárboles con sus respectivas claves, subclaves y entradas de valor
> se almacena en archivos binarios en disco
> cada elemento del Registro tiene un propietario,ACL y controles de auditoría

#### HKLM

> hardware, software , SO
> Memoria del sistema,controladores de dispositivos y datos de control

#### HKCR

> asociaciones de archivos
> También contiene la base de datos del registro OLE, el antiguo
> REG.DAT de Win 3.x

#### HKCU

> perfil del usuario que ha iniciado la sesión actual
> incluye las variables de entorno, la configuración del escritorio, las
> conexiones, impresoras y las preferencias para los programa

#### HKU

> Contiene una entrada para cada usuario que haya iniciado una sesión
> previamente en la computadora
> usuarios con acceso remoto al servidor no tienen perfiles en esta
> clave del servidor. Sus perfiles se cargan en el Registro de sus propios equipos

#### HKCF

> configuración de hardware del equipo local al iniciarse
> configurar opciones como los controladores de dispositivo resolución
> de pantalla

### REGEDIT

`HKCU\Software\Microsoft\Windows\CurrentVersion\Run` ← Quitar permisos.

`Process monitor.exe` ← Ver registro en tiempo real

`Operation: RegSetValue ← Jump to` ← Buscar donde cambian los valores / ir

`@echo off` ← crear .bat para ejecutarlo

`reg add HKCU\Software\microsft\notepad /v 1fFaceName /d “MV Boli” /f >nul`

mostrar todo en en carpeta > cargar subinterfaz > `NTUSER.DAT` ← añadir usuario a HKU

### SERVICIOS

> Los servicios son procesos que se ejecutan de forma automática al iniciar Windows y quedan residentes en memoria, proporcionando diversas funcionalidades con la mínima intervención del usuario.
> Proceso → Programa en ejecución Programa → archivo en el Disco Duro Programa (HD) → Proceso (RAM)

### VISOR DE EVENTOS

```bash
COMPMGMT.MSC # Visor de Eventos
Crear vista personalizada por origen # Crear regla
Vista personalizada > nombre > Guardar todos los eventos...
Secpol > Directivas locales > directiva de auditoría > objetos > activar →
carpeta > seguridad > Op Av > activar auditoría
```

### CUOTAS

> HABILITAR TODO VALORES DE CUOTRA = EDITAR CADA USUARIO

### ENCRIPTAR

```bash
PROPIEDADES > Atrib Avanzados > # Cifrar contenido para proteger datos 
> CERTMGR.MSC > ver claves ( y exportar)
```