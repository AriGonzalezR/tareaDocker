## EJERCICIO 1 - Trabajo con Imágenes

### 1.1- Servidor Web

Arrancamos un contendedor en Docker en el que ejecuto una instancia de la imagen php:7.4-apache, el contenedor se llamará web y será accesible desde un navegador por el puerto 8000.

Además he creado un volumen  con la opción `-v` con la ubicación en nuestra máquina cliente en. /home/arantzazu/web y que se sincroniza con una carpeta de nuestro contenedor ubicada en /var/www/html. Así cualquier cambio que hagamos en nuestro contenedor o nuestra máquina va a verse reflejados en ambos.

```
docker run -d --name web -v /home/arantzazu/web:/var/www/html -p 8000:80 php:7.4-apache
```

![1.1](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.1.png)

Resultado de la ejecución ![1.2](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.2.png)

Comprobamos que se ha creado el contenedor con :

```
docker ps -a
```

![1.3](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.3.png)

Entramos en el contenedor y listamos los archivos que tenemos en las carpeta /var/www/html

```
docker exec -it web bash
```

una vez dentro del contenedor con `ls`, listamos los archivos que tenemos, en este caso tendremos index.html y mes.php

![1.4](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.4.png)

Captura de pantalla de mi carpeta en la máquina cliente con los archivos solicitados y que está sincronizada con la carpeta del contenedor.

![1.9](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.9.png)

Ahora seguimos dentro del contenedor y nos vamos a la carpeta etc/apache2/mods-enabled  para instalar php7 y comprobamos que **php7.load** está en mods-enabled y hacemos un `restart` a apache

```
apt-get install php7 libapache2-mod-php7 php7-cli
```

![](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.6.png)

![1.7](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.7.png)

Comprobamos desde un navegador que se ejecuta el script del archivo mes.php . Tiene que salir el numero correspondiente al mes en el que estamos.

![1.8](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.8.png)

El codigo php fue creado en Visual Studio Code y es el siguiente:

![1.10](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.10.png)

El codigo para el archivo index.html fue creado en Visual Studio Code y es el siguiente:

![1.11](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.11.png)

y la salida en el navegador es la siguiente:

![1.12](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.12.png)

El contenedor Docker creado es el siguiente:

![1.13](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.13.png)

Ahora borramos el contenedor :

```
docker rm web
```

IMPORTANTE: Lo borro al final del ejercicio 1.2, porque así muestro en una captura los dos contenedores creados para estos dos ejercicios



