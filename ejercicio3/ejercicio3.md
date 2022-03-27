> Arantzazu González Rodríguez
>
> DAW La Laboral - Tarea 3 Despliegue

## Ejercicio 3 - Redes

Creamos una red bridge, llamada <u>redbd</u>

```
docker network create redbd
```

![3.1](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.1.png)

como no hemos indicado ninguna configuración en la red que hemos creado, docker asigna un direccionamiento a la red:

```
docker network inspect redbd
```

![3.2](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.2.png)

Ahora creamos un contenedor con una imagen de mariaDB que estará en la red **redbd**.

Se ejecutará en segundo plano (-d) y será accesible desde el puerto 3306. Con el usuario root con contraseña root y con un volumen de datos persistente (-v).

```
docker run -d --name mibase --network redbd -p 3306:3306 -v /opt/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=arantxa mariadb
```

![3.3](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.3.png)

Ahora creamos otro contenedor con una imagen Adminer , que se pueda conectar al contenedor **mibase** ( con la imagen mariadb) anteriormente creado ( lo hago a través de `--link` ). Los dos contenedores tienen que estar en la misma red (`--network redbd`). El puerto no he puesto el 8080, como pone la documentacion de la imagen de Adminer de Docker Hub, sino el 8083, porque por el puerto 8080, me daba un error que no pude subsanar por mucho que lo he intentado.

```
docker run --named adminer-ari --link mi_base:db --network redbd -p 8083:8080 -d adminer

```

![3.4](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.4.png)

Captura de los dos contenedores creados

```
docker ps 
```

![3.5](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.5.png)

Ahora abrimos una ventana del navegador firefox, accedo a :

http://localhost:8083 y podemos ver el interface de Adminer  4.8.1

![3.13](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.13.png)

Captura de la interfaz de Adminer, donde se puede ver la base de datos creada ( base de datos: arantxa) :

![3.12](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.12.png)



Accedo al contenedor creado con mariadb y llamado **mibase**

```
docker exec -it mibase bash
```

![1.13](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\1.13.png)

Accedo a la base de datos (con usuario root)

```
mysql -u root -p
```

Nos pide contraseña:

![3.9](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.9.png)

Ahora vemos la base de datos creada:

```
show databases;
```

![3.10](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.10.png)

Salimos del contenedor:

```
exit

exit
```

![3.11](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.11.png)

Ahora detenemos y eliminamos los contenedores:

```
docker stop $(docker ps -a -q)
```

![3.14](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.14.png)

```
docker rm $(docker ps -a -q)
```

![3.15](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.15.png)

(* con `docker ps -a` , comprobamos que ya no hay contenedores)

Ahora enumeramos los volumenes creados:

```
docker volume ls
```

![3.16](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.16.png)

Y lo eliminamos ( con este comando eliminamos todos los volúmenes existentes, yo tenía otro creado de prueba llamado mi_volumen y con esto lo eliminará también):

```
docker volume prune
```

![3.17](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.17.png)

Eliminamos la red creada, primero la visualizo ( la red creada se llamaba **redbd**:

```
docker network ls
```

![3.18](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.18.png)

Y ahora la borro:

```
docker network rm 9e74f6d750a6
```

![3.19](C:\Users\lasui\Documents\tareaDocker\CAPTURAS\3\3.19.png)

