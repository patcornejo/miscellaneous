# Python 3.11 en AWS EC2

El objetivo es poder instalar la versión de Python 3.11 en una instancia EC2 Amazon Linux.

- Se debe crear una instancia EC2 con Amazon Linux 2. Debe ser una t2.micro al menos.
- Se asume que estamos en la carpeta del usuario ec2-user (~).

Se realiza actualización de las librerías.

```bash
sudo yum update -y 
```

Se instalan las librerías de desarrollo para el OS.

````bash
sudo yum groupinstall "Development Tools" -y
````

Se descarga la versión 3.11 de Python que se desea instalar. Es posible obtener la URL de descarga para la versión gzip desde [aquí](https://www.python.org/downloads/source/).

```bash
wget https://www.python.org/ftp/python/3.11.1/Python-3.11.1.tgz
```

Se descomprime el archivo y se ingresa a la carpeta Python

````bash
tar xzf Python-3.11.1.tgz
cd Python-3.11.1
````

Se instalan librerías adicionales que necesita el maker para compilar Python. 

````bash
sudo yum install xz-devel gdbm-devel bzip2-devel libffi-devel tk-devel ncurses-devel readline-devel uuid zlib-devel -y
````

Se desinstala la version de OpenSSL que viene predeterminado. Esto debido a que Python 3.11 necesita OpenSSL 1.1.1 para compilar SSL y la versión que viene por defecto en Amazon Linux es la 1.1.0.

````bash
sudo yum remove openssl openssl-devel
sudo yum install openssl11 openssl11-devel
````

Se instala además mod_ssl, la interface Apache para SSL

```bash
sudo yum install mod_ssl
```

Se deja la versión openssl11 como predeterminada para el OS. De esta manera evitamos problemas al utilizar OpenSSL y podremos ejecutar el comando *openssl* desde el terminal.

```bash
sudo mv /usr/bin/openssl11 /usr/bin/openssl
```

Revisamos que openssl fue movido correctamente y está la versión 1.1.1 funcionando.

```bash
openssl version
```

Ejecutamos el configurador de Python 3.11 activando las optimizaciones, esto permite optimizar python en un 10% (más menos).

```bash
./configure --enable-optimizations
```

Una vez finalizado, ejecutamos *make* e instalamos en el OS. Esto debe hacerse con **sudo** o creando un ambiente para utilizar la versión 3.11. En este caso lo haremos con sudo para que quede a nivel de instancia y ambiente global.

```bash
sudo make install
```

Revisamos que se haya instalado la versión, debería devolvernos la versión 3.11 instalada. Ejecutamos el comando:

```bash
python3.11 --version
```

Deberíamos ver instalada la versión Python 3.11.x. Es importante entender que ahora la instancia tiene 3 versiones de Python: **python, python3 y python3.11**.
