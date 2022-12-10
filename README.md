# Contenedores
Los contenedores son un paquete de software estándar, que agrupa el código de una aplicación con las bibliotecas y los archivos de configuración asociados, junto con las dependencias necesarias para que la aplicación se ejecute. Esto permite a los desarrolladores y profesionales de TI implementar aplicaciones sin problemas en todos los entornos.
Los contenedores prometen resolver los problemas en el desarrollo de software al construir, distribuir y ejecutar.
Un contenedor es una agrupación de procesos, entidad lógica. Los procesos que se ejecutan en los contenedores sólo tienen rango de acción dentro del espacio del contenedor que se ejecutan. Los contenedores y el OS, comparten sólo el kernel.


## Contenedores y Maquinas Virtuales

StackEdit stores your files in your browser, which means all your files are automatically saved locally and are accessible **offline!**

## Create files and folders

- Virtualización, Son equipos ejecutando dentro de otra. Las Mv, son pesadas, virtualizan toda OS, son de administración costosa, son lentas al momento de inicializarlas, incia todo el OS.
![enter image description here](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/what-is-a-container_valprop1?resMode=sharp2&op_usm=1.5,0.65,15,0&qlt=75)
- Los contenedores son flexibles, livianos, portables, bajo acoplamiento, escalables y seguros. Son versátiles, ya que son ligeras y contienen todas las dependencias para su ejecución, garantizando su ejecución en todos lados.

![enter image description here](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/what-is-a-container_valprop2?resMode=sharp2&op_usm=1.5,0.65,15,0&qlt=75)

## Primeros pasos

Instalación de contendores [docker](https://docs.docker.com/engine/install/).
`docker run hello-world`

[Docker Cheat sheet](https://collectednotes.com/barckcode/docker-cheat-sheet)

## Uso de contenedores

-   `docker ps -a`, para listar todos los contenedores activos y no.
-   `docker inspect nombreoIDcontainer`, para inspeccionar un contenedor en particular.
-   `docker inspect -f '{{ json .Config.Env }}' nombreoIDcontainer`, para filtrar por un parámetro en particular, en este caso, las variables de entorno.
-   `docker rename oldname newname`, para renombrar un contenedor.
-   `docker run --name nombre imagecontainer`, para correr un contener con un nombre en particular.
-   `docker logs nombreIDcontainer`, para ver los logs de un container
-   `docker rm nombreIDcontainer`, para eliminar un contenedor.
-   `docker rm $(docker ps -aq )`, para eliminar todos los contenedores.
-   `docker run -it ubuntu`, para ejecutar una imagen de docker en modo interactivo.
-   `docker run ubuntu tail -f /dev/null`, para mantener nuestro conteneder en ejecución constante.
-   `docker exec -it nombrecontenedor comando`, para ejecutar un comando de manera iteractiva en el contenedor.

> 💡 docker, siempre asigna el PID 1 al proceso que ejecuta cuando crea el contenedor.

-   `docker kill nombrecontenedor`, para apagar el contenedor o hacer kill del proceso del contenedor.
-   `docker rm -f nombrecontendor`, para forzar la eliminación del contenedor.
-   Los contenedores no sólo están aislados a nivel de procesos, sino también a nivel de red y cada uno de ellos maneja su propio stack.
-   `docker run -d --name server ngnix -p 8080:80`, para crear un nuevo container de imagen _ngnix_, de nombre _server_, que no muestre el output del comando que ejecuta, y que el puerto _80_ del container sea redirigido al puerto _8080_ del hosts.
-   Para la persistencia de datos en los contenedores podemos montar para de nuestro directorio como volúmenes del filesystem de docker.
-   `docker run -d -name db mongo -v /home/user/mypath:/data/db` , creamos un nuevo container de imagen _mongo_, de nombre _db_, que no muestre el output del comando que ejecuta y que cree un volumen de la ruta _/home/user/mypath_ del host hacia la ruta _/data/db_ del container.
-   Las tres formas de persistencia de datos en docker son _blind mount_, _volume_ y _tmpfs mount_.
-   _volume_, son administrados por _docker_, se encuentran en _/var/lib/docker/volumes/_ en linux, Sólo los procesos de docker debería de poder modificar esta parte del filesystem.
-   _blind mount_, Son almacenados en cualquier lugar del sistema y los cualquier proceso puede tener acceso para realizar modificaciones.
-   _tmpfs mount_, son guardados en la memoria del sistema. Usados para guardar los datos durante la vida del contenedor.
-   `docker volume`, para ver los volumenes creados por docker en el OS.
-   `docker volume prune`, para depurar volumen que ya no se usan.
-   `docker volume create namevolume`, para crear un nuevo volumen de nombre namevolume, para ser usado por docker.
-   `docker run -d --name db --mount src=namevolume,dst=/data/db mongo`, creamos un nuevo container de imagen _mongo_, de nombre _db_, que no muestre el output del comando que ejecuta y que use el volumen _volumename_ del host ligado con la ruta _/data/db_ del container.[[+](https://docs.docker.com/storage/volumes/#backup-restore-or-migrate-data-volumes)]

![enter image description here](https://docs.docker.com/storage/images/types-of-mounts-volume.png)



## Imágenes

-   Las imágenes en docker, son templates de contenedores, que son inmutables. Una vez creada no la podemos cambiar.
-   Las imágenes cuentan _tags_, que es la forma en la que podemos saber cual es la versión de nuestra imagen.
-   Las imágenes se componen de varias capas, no es único archivo. Cada capa es inmutable, cada capa representa el cambio que ha tenido la imagen desde la capa base, muy parecido al _GIT_.
![enter image description here](https://imgs.search.brave.com/Kh1OwEBAFHL2DOKAk483Tb_ZI4NNkHO2REmfxnKBNHk/rs:fit:583:225:1/g:ce/aHR0cHM6Ly90c2U0/Lm1tLmJpbmcubmV0/L3RoP2lkPU9JUC5B/N2Yxa0RfNVlFTmh6/UjJMV0s4eUVBSGFH/QiZwaWQ9QXBp)

-   `docker image ls`, para listar las imágenes que tenemos
-   `docker pull imagename`, para traer una imagen de docker hub hacia nuestro sistema.
-   `docker pull imagename:tag`, para traer la imagen con un tag específico.
-   `docker rmi image_name:version/image-id`, para eliminar una imagen especifica.
-   `docker rmi $(docker images -qf "dangling=true"),` para eliminar todas las imágenes.
-   `docker rmi $(docker images | grep -v ‘ubuntu|my-image’ | awk {‘print $3’})`, eliminar todas las imagnes exepto ubuntu|my-image
-   `docker rmi $(docker images --quiet | grep -v $(docker images --quiet ubuntu:my-image))`, eliminar todas las imagenes exepto `ubuntu:my-image`
-   Para crear imágenes en docker usamos los _Dockerfile_.
    -   `FROM`, nos indica la imagen o la capa base que usaremos para crear nuestra imagen.
    -   `RUN`, para ejecutar comandos en nuestra imagen base.
-   `docker built -t imagename:tag .`, para construir nuestra imagen, a partir del docker file que se encuentra en el contexto de build `.` , el nombre de la imagne tag `imagename:tag`.
-   `docker tag imagename:tag <user_dockerhub>/imagename:tag`, para etiquetar la imagen creada a mi repositorio en dockerhub
-   `docker push <user_dockerhub>/imagename:tag`, para mandar la imagen a nuestro repositorio.
-   Cuando se realiza el _push_, sólo se envías los cambios respecto a la imagen base que se encuentra en el repositorio.[[1](https://www.docker.com/blog/how-to-use-your-own-registry/),[2](https://docs.docker.com/docker-hub/repos/)]
-   `docker history --no-truc imagename:tag`, para revisar el historial de las capas para la generación de una imagen.
-   `dive imagename:tag`, para ver de manera más detallada el historial de capas de una imagen.[[+](https://github.com/wagoodman/dive)]. Ctrl-U, para ver los archivos modificados.

> 💡 `FROM scratch`, es la base para imágenes linux, que sólo contiene  el kernel del OS.

## Desarrollo con Docker

-   `COPY [".","/usr/src"]`, para copiar archivos de un origen, en este caso del contexto de *build* **"."**, hacia un destino **"/usr/src"**
-   `WORKDIR /usr/src`, es como un `cd` de la línea de comandos hacia el directorio base.
-   `EXPOSE 3000`, para abrir o exponer el puerto, en  este caso _3000_.
-   `CMD ["node","index.js"]`, Es el comando que se ejecutará cuando se inicie el contenedor.

> 💡 Si bien la descarga de las capas de la imágenes de docker es en simultaneo, la extracción de las mismas se ejecuta de manera secuencial.

-   `docker run -rm -p 3000:3000 imagename`, para eliminar el contenedor, cuando termine el ciclo del o contenedor o se apague.

```docker
FROM node:10
COPY [".","/usr/src"]
WORKDIR /usr/src
RUN npm install
EXPOSE 3000
CMD ["node","index.js"]
```

-   En docker, cuando se vuelve a ejecutar el comando **build** del *dockerfile*, este usa su cache sobre las capas que no han sufrido cambios, optimizando la reconstrucción de la imagen.
-   En nuestro caso podemos optimizar nuestro dockerfile de la siguiente manera. Copiando sólo los archivos necesarios para la instalación de dependencias y el resto de archivos al final.

```docker
FROM node:10
COPY ["package.json","package-lock.json","/usr/src"]
WORKDIR /usr/src
RUN npm install
COPY [".","/usr/src"]
EXPOSE 3000
CMD ["node","index.js"]
```

-   Para no tener que apagar el container cada vez que realizamos un cambio, Node cuenta con una utilidad llamada nodemon, el cual permite reiniciar el servicio de node cada vez que detecta un cambio en los archivos de node. Para ello cambiamos `CMD ["node","index.js"]` por `CMD ["npx", "nodemon","index.js"]`
-   `docker network ls`, nos permite listar las redes disponible para los contenedores.
-   El driver por defecto de docker es _brigde_, pero también se puede usar el driver _host_ que permite que usar la red del _host_ y _none_ que no cuenta con conexión de red.
-   `docker create network --attachable namenet`, para crear una red que permita que más container se conecten a él.
-   `docker network connect namenet imagename`, para conectar una imagen a una red.
-   `docker network inspect namenet`, para inspeccionar y ver los contenedores conectados a la namenet
-   `doker run -d --name app -p 3000:3000 --env MONGO_URL=mongodb://db:27027/test app`, iniciamos nuestra imagen app con variables de entorno, llamando a la imagen de mongo que ya tenemos iniciada de nombre **db**.
