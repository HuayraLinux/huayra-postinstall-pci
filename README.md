# Huayra-Postinstall PCI

La intención del paquete es poder terminar de configurar la última versión de Huayra las netbooks del Programa Conectar Igualdad que fueron entregadas con una anterior u otra distribución de GNU/Linux. 

Por favor, *LEA ATENTAMENTE TODA LA DOCUMENTACIÓN* para conocer que implicancia tiene el uso de
éste paquete.

Se recomienda ejecutarlo inmediatamente después de la instalación de Huayra GNU/Linux

## Detalle

Éste paquete agrega el comando **huayra-postinstall-pci**

Al ejecutar el comando, se lanzan una serie de scripts que modifican configuraciones de la netbook para dejarla similar a las que vienen de fábrica.

Por ejemplo, se agrega al fstab las particiones de DATOS y RECURSOS (ésta última en el caso de imágenes nuevas >= 2014).

Así como también los links symbólicos de las carpetas de usuario (Documentos, Imágenes, Música, etc) a la partición de DATOS entre otras acciones.

Antes de ejecutarlo debe conocer cual es su versión de imágen que vino con la netbook. Para que las configuraciones se adapten segun sea el caso.

Actualmente éste script puede configurar netbooks que salieron con imagenes 2013 y 2014 respectivamente 

### 2013

Son las netbooks entregadas durante el 2013 con Huayra 1.1. Estas contienen la tabla de 
particiones similar a la siguiente (puede obtener ésta información haciendo sudo fdisk -l desde una terminal):

```
Device     Boot     Start       End   Sectors   Size Id Type
/dev/sda1  *         2048  83890175  83888128    40G 83 Linux
/dev/sda2        83891430 167782859  83891430    40G  7 HPFS/NTFS/exFAT
/dev/sda3       167782921 625141759 457358839 218,1G  f W95 Ext'd (LBA)
/dev/sda5       167782923 209728574  41945652    20G 83 Linux
/dev/sda6       209728638 216023039   6294402     3G 82 Linux swap / Solaris
/dev/sda7       216026118 625141759 409115642 195,1G  b W95 FAT32
```
*sda1*: Partición raiz donde se encuentra instalado el actual GNU/Linux

*sda2*: Partición donde está instalado windows

*sda3*: Partición extendida de DATOS

*sda5*: Es la particion que contiene el sistema de recuperación de imágenes

*sda6*: Partición de intercambio (swap)

Contienen un directorio llamado "Mis Cosas" en la particion de DATOS donde se encuentran los archivos y docuementos del usuario.

### 2014

Son las entregadas con la plataforma MarblePoint dusrante el 2014, tienen la particularidad de que se puede rotar la cámara web, bios EFI y contenían Huayra 2.x

La tabla de particiones es GPT y similar a la siguiente (para visualizarla debe tener instalado el paquete gdisk y hacer sudo gdisk -l /dev/sda):

```
GPT fdisk (gdisk) version 0.8.5

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: present

Found valid GPT with protective MBR; using GPT.
Disk /dev/sda: 625142448 sectors, 298.1 GiB
Logical sector size: 512 bytes
Disk identifier (GUID): 6B5AAF14-C6FF-4C69-8B16-45724E6F9BB1
Partition table holds up to 128 entries
First usable sector is 34, last usable sector is 625142414
Partitions will be aligned on 2048-sector boundaries
Total free space is 2669 sectors (1.3 MiB)

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048        83888127   40.0 GiB    8300  
   2        83888128        84502527   300.0 MiB   2700  Basic data partition
   3        84502528        84707327   100.0 MiB   EF00  EFI system partition
   4        84707328        84969471   128.0 MiB   0C01  Microsoft reserved part
   5        84969472       168962047   40.1 GiB    0700  Basic data partition
   6       168962048       210905087   20.0 GiB    8300  
   7       210905088       219293695   4.0 GiB     8200  
   8       219293696       292694015   35.0 GiB    0700  
   9       292694016       625141759   158.5 GiB   0700  
```

*1*: Partición raiz de GNU/Linux

*3*: Partición de arranque EFI (fat32)

*5*: Instalación de windows

*6*: Sistema de recuperación

*7*: Partición de intercambio (swap)

*8*: Partición de RECURSOS (solo lectura)

*9*: Partición de DATOS

## Modo de uso

Una vez detectado que imagen tiene su netbook (2013 o 2014) se puede realizar una prueba de los scripts antes de modificar los datos con el parámetro -t (test)

*NOTA*: Debe ejecutarlo como usuario *alumno* (no como usuario root), en caso que se necesite acceso root, se le pedirá la clave del mismo cuando sea conveniente.  

```
$ huayra-postinstall -t 2014
```
Si todo está correcto podemos ejecutar finalmente el comando para que realice los cambios en el equipo con el parámetro -r (run):

```
$ huayra-postinstall -r 2014
```
Luego de finalizar este proceso, se debe reiniciar el equipo para que se apliquen todos los cambios.

Para ver el código de los scripts, se puede ingresar a /usr/share/huayra-postinstall-pci/ donde están los directorios **2013.d** y **2014.d** respectivamente. Aquí están cada uno de los archivos que se ejecutan.

Por favor si encuentra algun error, notifíquelo en http://bugs.huayra.conectarigualdad.gob.ar
