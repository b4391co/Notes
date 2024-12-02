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
