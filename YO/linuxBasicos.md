<h1 style="text-align:center;font-size:40px;">LINUX Basico</h1>

## instalación

### kali

[kde](https://computingforgeeks.com/install-kde-desktop-environment-on-kali-linux/)

### mostrar ips

[publica](https://github.com/jmisasa/xfce-my-ip)
[privada](https://github.com/NICKCHEESE/Kali-Toolbar-IP)

### [activar bluetooth](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1009987)

```sh
mkdir /var/lib/bluetooth
systemctl start bluetooth.service
```

### abrir apps remotas

```sh
xfreerdp /v:IP /u:user /cert-ignore /dynamic-resolution +auto-reconnect +clipboard +home-drive /app:explorer
```

## [Winapps](https://github.com/Fmstrat/winapps)

```sh
export LIBVIRT_DEFAULT_URI=qemu:///system
```

## Soluciones

[Anydesk error libpangox](https://www.youtube.com/watch?v=47ZgIsj1jMc)

### Nvidia arch

[GUIA](https://www.youtube.com/watch?v=AOjOd3wIPu8)

### Grub Windows

```sh
menuentry "Windows" {
           insmod ntfs
           search --fs-uuid --no-floppy --set=root 7421-C6A8
           chainloader /efi/Microsoft/Boot/bootmgfw.efi
}
```

### libcurl

```sh
wget https://archive.archlinux.org/packages/c/curl/curl-7.63.0-3-x86_64.pkg.tar.xz
tar -xvf curl-7.63.0-3-x86_64.pkg.tar.xz
sudo cp usr/* /usr  -r
```

### Firmas gpg (yay)

```sh
gpg --refresh-keys
sudo pacman -Scc

sudo rm -r /etc/pacman.d/gnupg
sudo pacman-key --init
sudo pacman-key --populate archlinux

 ```

<h1 style="text-align:center;font-size:40px;">EXTRA</h1>

## vim

```sh
:%s/a_sustituir/nueva/g ← reemplazar
```

## VBox

### Montar GuestAdditions

```sh
mount /dev/cdrom /mnt
./VBoxLinuxAdditions.run
```

### Permisos

```sh
usermod -a -G vboxsf "$(whoami)" # grupo
mount -t vboxsf NombreOriginal /mnt/x # montar
```

## MSG

```sh
zenity --info --text="mensaje"
```

> si no funciona	1	2

```sh
export DISPLAY=:0.0
xmessage -display :X "Text"
for i in {1..x}; do COMANDO; done
```

## no sync users

```sh
systemctl stop winbind
systemctl restart nmbd
systemctl start winbind
```

## OMITIR TPM WIN 11

### ISO

```sh
regedit
HKLM/SYSTEM/Setup nueva clave: labconfig
nuevo valor DWORD 32 bits
BypassTPMCheck		1
BypassSecureBootCheck	1
```

## OPTIMIZAR

```sh
iex((New-Object System.Net.WebClient).DownloadString('https://git.io/debloat'))

iwr -useb https://christitus.com/win | iex
```

### Windows defender

```sh
Get-MpPreference | Select ScanAvgCPULoadFactor
Set-MpPreference -ScanAvgCPULoadFactor 20
gpupdate /force
```

### SERVICIOS

```sh
Administrador de cuentas web // cola de impresion // control parental
Fax // printworkflow // s. de asistente para la compatibilidad
s. de directivas de diagnostico // s. de instalacion de ms store
s. orquestador de actual.. // walletservice // Windows search
windows update // xbox accessory //
```

### CONFIGURACION WINDOWS

```sh
SISTEMA > ACTIVAR > ALMACENAMIENTO > DESACTIVAR
Notificaciones // asistente // multitarea
```

### MSCONFIG

> activar todos los procesadores
