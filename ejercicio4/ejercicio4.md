## EJERCICIO 4 - Imagen con DockerFile

Creo una carpeta en Home llamada <u>dockerweb</u>

```
mkdir ~/dockerweb/
```

![4,1](../CAPTURAS/4/4,1.png)

creamos un archivo llamado Dockerfile:

```
nano ~/dockerweb/Dockerfile
```

![3](../CAPTURAS/4/òtro/3.png)

Explicando el contenido del Dockerfile:

- debian: es el sistema operativo donde se van a montar los programas
- RUN apt-get update: aplicaciones a instalar o actualizar
- ADD public_html /var/www/html/: copiar del directorio home al directorio de apache del contenedor
- expose 80: puerto que va a quedar abierto
- CMD /usr/sbin/apache2vtl -D FOREGROUND: ejecutar en la terminal como demonio.

Ahora ejecutamos en docker el contenido de Dockerfile:

```
sudo docker build -t web ~/dockerweb/
```

donde :

- web : es el nombre de la nueva imagen que crearemos
- ~/dockerweb/: Path donde está nuestro Dockerfile.

![1](../CAPTURAS/4/òtro/1.png)

![2](../CAPTURAS/4/òtro/2.png)

en nuestras imagenes de docker vamos a ver la nueva imagen creada:

```
docker images
```

![4](../CAPTURAS/4/òtro/4.png)

ahora creamos el contenedor con la imagen creada y que hemos llamado **web**

```
docker run -d -p 8086:80 --name arantxa web:latest
```

Si abrimos en una página de un navegador, vemos la web desplegada. Escribimos http://localhost:8086

![5](../CAPTURAS/4/òtro/5.png)

Hacemos clic en el botón contacto y accedemos a la página "/contacto"

![6](../CAPTURAS/4/òtro/6.png)

Captura de pantalla de las carpetas en el host cliente:

![8](../CAPTURAS/4/òtro/8.png)

![7](../CAPTURAS/4/òtro/7.png)

Solamente nos quedaría subir nuestra imagen a Docker Hub. Pero primero nos aseguramos de estar en la carpeta correcta:

```
cd /home/arantzazu/dockerweb/
```

listamos el contenido de esta carpeta, para asegurarnos que sea la correcta

```
ls
```

![9](../CAPTURAS/4/òtro/9.png)

Ahora solo quedaría subir la imagen a DOCKER HUB

```
docker login

```

![4.9](../CAPTURAS/4/4.9.png)

Etiqueto la construcción de mi imagen

```
docker tag web:latest arigonzalezr/web:ari
```

y ahora subo la imagen

```
docker push arigonzalezr/web:ari
```

![4.10](../CAPTURAS/4/4.10.png)

Muestro mi imagen subida a DockerHub

![4.11](../CAPTURAS/4/4.11.png)
