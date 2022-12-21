# Instalar Asterisk v20 + PJSIP EC2 Amazon Linux

Se asume que se levantó una instancia EC2 con Amazon Linux 2022 (al2022).

### Revisar SELINUX
Primero es necesario revisar que **SELINUX** esté deshabilitado.

````shell
vi /etc/selinux/config
````

Revisar dentro del archivo que esté deshabilitado:

````
SELINUX=disabled
````

### Instalar Paquetes Requeridos

```shell
sudo yum install -y dmidecode gcc-c++ ncurses-devel libxml2-devel make wget openssl-devel newt-devel kernel-devel sqlite-devel libuuid-devel gtk3-devel jansson-devel binutils-devel libedit libedit-devel
```
### Instalar PJSIP

Primero se crea una carpeta temporal para compilar PJSIP

````shell
mkdir ~/build && cd ~/build
````

Descargar y extraer la versión de PJSIP 2.13

````shell
wget https://github.com/pjsip/pjproject/archive/refs/tags/2.13.tar.gz
tar -zxvf 2.13.tar.gz
cd pjproject-2.13/ 
````

Preparar el paquete para compilar:

````shell
./configure CFLAGS="-DNDEBUG -DPJ_HAS_IPV6=1" --prefix=/usr --libdir=/usr/lib64 --enable-shared --disable-video --disable-sound --disable-opencore-amr
````
![pjsip configure](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/pjsip_configure.png)

Revisar que las dependencias esten ok:
````shell
make dep
````
![pjsip make dep](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/pjsip_make_dep.png)

Completar la instalación:

````shell
make && sudo make install && sudo ldconfig
````
![pjsip sudo ldconfig](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/pjsip_sudo_ldconfig.png)

Revisar que la instalación resultara bien:

````shell
ldconfig -p | grep pj
````
![pjsip grep pj](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/pjsip_grep_pj.png)

### Instalar Asterisk

Volver a la carpeta build

````shell
cd ~/build
````

Descargar la última versión LTS. Se puede revisar desde https://www.asterisk.org/downloads/.

````shell
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-20-current.tar.gz
````

Extraer y navegar a la carpeta de la versión.

````shell
tar -zxvf asterisk-20-current.tar.gz
cd asterisk-20.0.1
````
Instalar librerías adicionales para features específicos de Asterisk.

````shell
sudo yum install patch unixODBC unixODBC-devel
````

Preparar la instalación ejecutando el script de configuración.

````shell
./configure --libdir=/usr/lib64 --with-jansson-bundled
````

Realizar la configuración deseada desde el menu de selección del instalador de Asterisk.

````shell
make menuselect
````
Se debe asegurar de que estén activados las siguientes características:

* CORE-SOUNDS-ES-GSM (para los audios en español)
* res_odbc, func_odbc y cdr_odbc para poder conectarse a una BDD MySQL / MariaDB.

Una vez seleccionada la configuración, presionar *Save & Exit*. Posteriormente instalar la configuración seleccionada.

````shell
make && sudo make install
````
![asterisk install complete](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/asterisk_install_ok.png)

Instalar los archivos de configuración de ejemplo.

````shell
sudo make samples
````

Para iniciar el servicio de asterisk:

````shell
sudo asterisk
````

Una vez iniciado podrás ingresar a la consola de Asterisk.

```shell
sudo asterisk -rvvvvv
```
![asterisk cli](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/asterisk_console_cli.png)
