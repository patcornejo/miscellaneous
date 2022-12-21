# Instalar NodeJS 18/19 en AWS EC2 - Amazon Linux 2022

Es común al intentar instalar una versión superior o igual a 18 de NodeJS en instancias AWS Linux 2 que aparezca el siguiente error:

```
node: /lib64/libm.so.6: version `GLIBC_2.27' not found (required by node)
node: /lib64/libc.so.6: version `GLIBC_2.28' not found (required by node)
```

Esto debido a que las instancias Amazon Linux 2 vienen con GLIBC versión 2.26 y sólo es posible instalar versiones node 17 hacia abajo.

## Instalar Instancia EC2 Amazon Linux 2022
Se puede instalar una instancia con la imagen **Amazon Linux 2022 Preview** para instalar alguna versión de node sobre la 18. Para esto:

Ingresar al Catálogo de AMI desde la región que deseas instalar.

![ami catalog 1](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/ami_catalog_1.png)

Realizar búsqueda ***al2022*** y seleccionar los resultados de la comunidad.

![ami catalog 2](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/ami_catalog_2.png)

Seleccionar la primera opción e instalar una nueva instancia con esta imagen Amazon Linux 2022 (al2022):

![ami catalog 3](https://gc-patcornejo.s3.sa-east-1.amazonaws.com/imgs/ami_catalog_3.png)

Una vez instalada, es posible instalar node a través de NVM

## Instalar NodeJS 18 / 19

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

