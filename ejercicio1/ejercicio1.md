## EJERCICIO 1 - Trabajo con Imágenes

### 1.1- Servidor Web

Arrancamos un contendedor en Docker en el que ejecuto una instancia de la imagen php:7.4-apache, el contenedor se llamará web y será accesible desde un navegador por el puerto 8000.

Además he creado un volumen  con la opción `-v` con la ubicación en nuestra máquina cliente en. /home/arantzazu/web y que se sincroniza con una carpeta de nuestro contenedor ubicada en /var/www/html. Así cualquier cambio que hagamos en nuestro contenedor o nuestra máquina va a verse reflejados en ambos.

```
docker run -d --name web -v /home/arantzazu/web:/var/www/html -p 8000:80 php:7.4-apache
```

![1.1](../CAPTURAS/1.1.png)

Resultado de la ejecución ![1.2](../CAPTURAS/1.2.png)

Comprobamos que se ha creado el contenedor con :

```
docker ps -a
```

![1.3](../CAPTURAS/1.3.png)

Entramos en el contenedor y listamos los archivos que tenemos en las carpeta /var/www/html

```
docker exec -it web bash
```

una vez dentro del contenedor con `ls`, listamos los archivos que tenemos, en este caso tendremos index.html y mes.php

![1.4](../CAPTURAS/1.4.png)

Captura de pantalla de mi carpeta en la máquina cliente con los archivos solicitados y que está sincronizada con la carpeta del contenedor.

![1.9](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.9.png)

Ahora seguimos dentro del contenedor y nos vamos a la carpeta etc/apache2/mods-enabled  para instalar php7 y comprobamos que **php7.load** está en mods-enabled y hacemos un `restart` a apache

```
apt-get install php7 libapache2-mod-php7 php7-cli
```

![](../CAPTURAS/1.6.png)

![1.7](../CAPTURAS/1.7.png)

Comprobamos desde un navegador que se ejecuta el script del archivo mes.php . Tiene que salir el numero correspondiente al mes en el que estamos.

![1.8](../CAPTURAS/1.8.png)

El codigo php fue creado en Visual Studio Code y es el siguiente:

![1.10](../CAPTURAS/1.10.png)

El codigo para el archivo index.html fue creado en Visual Studio Code y es el siguiente:

![1.11](../CAPTURAS/1.11.png)

y la salida en el navegador es la siguiente:

![1.12](../CAPTURAS/1.12.png)

El contenedor Docker creado es el siguiente:

![1.13](../CAPTURAS/1.13.png)

Ahora borramos el contenedor :

```
docker rm web
```

IMPORTANTE: Lo borro al final del ejercicio 1.2, porque así muestro en una captura los dos contenedores creados para estos dos ejercicios

### 1.2- Servidor Base de Datos

Descargo la imagen de docker de MariaDB desde el repositorio en línea.

```
docker pull mariadb
```

captura de la ejecución

![1.2.1](../CAPTURAS/1.2/1.2.1.png)

Enumero las imagenes de docker instaladas en el sistema:

```
docker images
```

![1.2.2](../CAPTURAS/1.2/1.2.2.png)

Inicio un nuevo contenedor de Docker de MariaDB con esta imagen de Docker:

```
docker run --detach --name bbdd --env MARIADB_USER=invitado --env MARIADB_PASSWORD=invitado --env MARIADB_ROOT_PASSWORD=root --env MARIADB_DATABASE=prueba mariadb:latest

```

Captura de la ejecución para la creación del contenedor:

![1.2.3](../CAPTURAS/1.2/1.2.3.png)

Comprobamos que se ha creado el contenedor con MariaDB:

![1.2.4](../CAPTURAS/1.2/1.2.4.png)

Me conecto al contenedor recién creado con una sesion shell usando el comando:

```
docker exec -it bbdd bash
```

![1.2.5](../CAPTURAS/1.2/1.2.5.png)

Lo primero que hago es actualizar Ubuntu.

```
apt update && upgrade -y
```

![1.2.6](../CAPTURAS/1.2/1.2.6.png)

Entramos en la base de datos con el comando:

```
mysql -u invitado -p
```

nos pedirá la contraseña ( no se ve según telceas, pero la contraseña es: invitado)

![1.2.7](../CAPTURAS/1.2/1.2.7.png)

Visualizamos la base de datos creada con el comando show databases:

```
show databases;
```

![1.2.8](../CAPTURAS/1.2/1.2.8.png)

Salimos de la base de datos con exit:

![1.2.9](../CAPTURAS/1.2/1.2.9.png)

Salimos del contenedor con exit:

![1.2.10](../CAPTURAS/1.2/1.2.10.png)

Visualizamos las imágenes que tenemos:

```
docker images
```

![1.2.11](../CAPTURAS/1.2/1.2.11.png)

Intento borrar la imagen de mariadb:

```
docker rmi mariadb
```

y vemos que no se puede borrar, mientras un contenedor esté usando la imagen:

![1.2.12](../CAPTURAS/1.2/1.2.12.png)

Ahora borramos ambos contenedores creados para los dos anteriores ejecicios:

Primero los visualizamos:

```
docker ps -a
```

![1.2.13](../CAPTURAS/1.2/1.2.13.png)![1.2.13](../CAPTURAS/1.2/1.2.13.png)

y luego los detenemos ( en el caso que esté UP y los eliminamos):

```
docker stop $(docker ps -a -q)
```

![1.2.14](../CAPTURAS/1.2/1.2.14.png)

```
docker rm $(docker ps -a -q)
```

![1.2.15](../CAPTURAS/1.2/1.2.15.png)

Comprobar que se han eliminado:

```
docker ps 

docker ps -a
```

![1.2.16](../CAPTURAS/1.2/1.2.16.png)

