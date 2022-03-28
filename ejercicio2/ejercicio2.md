Arantzazu González Rodríguez

DAW La Laboral

## EJERCICIO 2 - Almacenamiento

### 2.1- Bind mount para compartir datos

Creo una carpeta llamada *saludo y dentro de ella un fichero llamado index.html:

```
mkdir saludo
cd saludo
echo "<h1> HOLA SOY ARANTZAZU GONZÁLEZ RODRÍGUEZ </h1>" > index.html

```

Captura del directorio del sistema del archivo del host, donde se ve la carpeta creada y el fichero `html`

![2.2](../CAPTURAS/2/2.2.png)

Visualizo las imagenes que tengo:

```
docker images
```

![2.3](../CAPTURAS/2/2.3.png)

Ahora creo un dos contenedores basados en la imagen php:7.4-apache que hagan un bind mount de la carpeta saludo en la carpeta /var/www/html del contenedor. Uno de ellos vamos a acceder con el puerto <u>8181</u> y el otro con el <u>8282</u> y sus nombres serán **c1** y **c2**

Creo el primer contenedor llamado c1 y accede por el puerto 8181

```
docker run -d --name c1 --mount type=bind,src=/home/arantzazu/saludo,dst=/var/www/html -p 8181:80 php:7.4-apache
```

![2.4](../CAPTURAS/2/2.4.png)

Creo el segundo contenedor llamado c2 y accede por el puerto 8282:

```
docker run -d --name c2 --mount type=bind,src=/home/arantzazu/saludo,dst=/var/www/html -p 8282:80 php:7.4-apache
```

![2.5](../CAPTURAS/2/2.5.png)

Pantallazo donde se pueden ver los dos contenedores creados:

```
docker ps -a
```

![2.6](../CAPTURAS/2/2.6.png)

Pantallazo donde se ve que accediendo a **c1** se puede ver el contenido de index.html

```
docker exec -it c1 bash
```

una vez dentro listamos contenido y vemos index.html :

```
ls
```

![2.7](../CAPTURAS/2/2.7.png)

y con el comando `cat` , podemos ver el contenido:

```
cat index.html
```

![2.8](../CAPTURAS/2/2.8.png)

salimos del contenedor 

```
exit
```

![2.9](../CAPTURAS/2/2.9.png)

Hacemos lo mismo para el contenedor **c2**

```
docker exec -it c2 bash
```

**repetimos los pasos de c1 en c2

![2.10](../CAPTURAS/2/2.10.png)

Paramos los contenedores:

```
docker stop $(docker ps -a -q)
```

![2.11](../CAPTURAS/2/2.11.png)

Modificamos el contenido de index.html

```
echo "<h1> ADIOS ARANTZAZU GONZÁLEZ RODRÍGUEZ </h1>" > saludo/index.html
```

Mostramos el contenido de index.html a través del navegador, accediendo a localhost:8181 y localhost:8282 , vemos que se ha cambiado el HOLA  por un ADIOS.

![2.12](../CAPTURAS/2/2.12.png)



![2.13](../CAPTURAS/2/2.13.png)

Reiniciamos los contenedores :

```
docker start c1

docker start c2
```

![2.14](../CAPTURAS/2/2.14.png)

Accedemos al contenedor **c1** para comprobar que los cambios en index.html también están en la carpeta del contenedor que lo contiene ( el archivo html, lo habíamos modificado en la máquina cliente)

```
docker exec -it c1 bash
```

Una vez dentro 

```
ls

cat index.html
```

y comprobamos que ahora el index.html en la cabecera dice "Adios " en vez de "Hola" como al principio.

![2.15](../CAPTURAS/2/2.15.png)

Hacemos lo mismo para el contenedor **c2**

```
docker exec -it c2 bash
```

![2.16](../CAPTURAS/2/2.16.png)

Ahora paramos los contenedores;

```
docker stop $(docker ps -a -q)
```

![2.17](../CAPTURAS/2/2.17.png)

y los eliminamos

```
docker rm $(docker ps -a -q)
```

![2.18](../CAPTURAS/2/2.18.png)

comprobamos que han sido eliminados:

```
docker ps -a
```

![2.19](../CAPTURAS/2/2.19.png)

