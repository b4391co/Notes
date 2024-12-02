<h1 style="text-align:center;font-size:40px;">ANÁLISIS FORENSE</h1>


## Volatility

```sh
volatility -f Triage-Memory.mem imageinfo # <-- sacar el perfil
volatility -f Triage-Memory.mem --profile=Win7SP1x64 pstree # listado de los procesos
volatility -f Triage-Memory.mem --profile=Win7SP1x64 netscan # escaneo de red 
volatility -f Triage-Memory.mem --profile=Win7SP1x64 netscan | grep -v -e 0.0.0.0 -e 127.0.0.1 -e :: -e -: # escaneo de red con filtros
volatility -f Triage-Memory.mem --profile=Win7SP1x64 dlllist # listado de DLLs
volatility -f Triage-Memory.mem --profile=Win7SP1x64 cmdline
volatility -f Triage-Memory.mem --profile=Win7SP1x64 procdump --pid=1136 --dump-dir . # Exportar un proceso
volatility -f Triage-Memory.mem --profile=Win7SP1x64 hashdump # hashes de los usuarios
volatility -f Triage-Memory.mem --profile=Win7SP1x64 mftparser # registro MFT

volatility -f caso1Volatility.dmp --profile=Win7SP1x64 envars | grep -e 'Pid' -e 'COMPUTERNAME' # Nombre del equipo
```



