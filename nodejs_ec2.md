# Instalar NodeJS 18/19 en AWS EC2 - Amazon Linux 2

Es común al intentar instalar una versión superior o igual a 18 de NodeJS en instancias AWS Linux 2 que aparezca el siguiente error:

```
node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by node)
```

Esto debido a que las instancias Amazon Linux 2 vienen con GLIBC versión 2.26 y sólo es posible instalar versiones node 17 hacia abajo.

Para proceder a instalar versiones superiores debemos:

## Actualizar GLIBC a 2.28

Para esto primero descargamos los paquetes de glibc 2.28:

````bash
wget https://vault.centos.org/centos/8/BaseOS/x86_64/os/Packages/glibc-2.28-164.el8.x86_64.rpm
wget https://vault.centos.org/centos/8/BaseOS/x86_64/os/Packages/glibc-common-2.28-164.el8.x86_64.rpm
wget https://vault.centos.org/centos/8/BaseOS/x86_64/os/Packages/glibc-devel-2.28-164.el8.x86_64.rpm
wget https://vault.centos.org/centos/8/BaseOS/x86_64/os/Packages/glibc-headers-2.28-164.el8.x86_64.rpm
wget https://vault.centos.org/centos/8/BaseOS/x86_64/os/Packages/nscd-2.28-164.el8.x86_64.rpm
````
Posteriormente, ejecutamos la instalación:

````bash
sudo rpm --force --nodeps -Uvh glibc-2.28-164.el8.x86_64.rpm glibc-common-2.28-164.el8.x86_64.rpm glibc-devel-2.28-164.el8.x86_64.rpm glibc-headers-2.28-164.el8.x86_64.rpm nscd-2.28-164.el8.x86_64.rpm
````

Esto instalar la versión glibc 2.28 en nuestra instancia, luego podemos instalar nodejs de la forma clásica.

## Instalar NodeJS

Descargar NVM:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
```
Activar NVM para su uso:

```bash
. ~/.nvm/nvm.sh
```
Finalmente, podemos instalar la versión LTS, una versión específica o la última disponible:
```bash
nvm install --lts
```

```bash
nvm install v18.12.1
```

```bash
nvm install node
```

Podemos comprobar que node se ha instalado correctamente:

```bash
node --version
```


## Notas importantes

* Ejecutar ***sudo yum update*** antes de realizar el proceso para no sobreescribir alguna actualización de AWS.
* El proceso está realizado para instancias con arquitectura x86_64, para ARM se deben descargar los paquetes que corresponde desde https://centos.pkgs.org.
* No realizar el proceso en modo **sudo**, excepto en la instalación de los paquetes rpm.
* **PRECAUCIÓN:** Es posible que la instancia deje de funcionar o que el módulo sudo sea imposible de utilizar.

# Opción 2

Se puede instalar la imagen **Amazon Linux 2022 Preview** en la instancia para instalar alguna versión de node sobre la 18. Para esto:

Instalar la última imagen de Amazon Linux 2022 (al2022):

![al2022 ami ec2](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/al2022_ec2.png)

Una vez instalada, es posible instalar node a través de NVM

## Notas Importantes

* Esta es la mejor opción para instalar NodeJS 18 o 19.

