# Docker:

- **docker images ls** : lista las imagenes
- **docker ps -a** : lista los containers
- **docker ps**: lista todos los containers corriendo
- **docker run <image>**: levanta una imagen del hub y la corre localmente
- **docker build** : crea una imagen a partir de un dockerfile
- **docker stop <CONTAINER_NAME>**: para la ejecucion del container
- **docker start <CONTAINER_NAME>**: levanta nuevamente un docker que corto la ejecucion, podemos verlo en docker ps -a
- **docker run -p 3000:80 <img_id>** : Crea un nuevo container a partir de una imagen en el puerto 80, el puerto donde esta publicando el servicio .
  este caso es: docker run -p <Puerto_app>:<Puerto_pc>

- **Docker run vs Docker start**: docker run ( crea un nuevo container ) setea la consola en modo atached, esto nos permite hacer pruebas, logs y demas, esto es porque "attached" significa que esta escuchando al output del container
  con docker start, el proceso queda trabajando en background. por lo que este nos permite seguir utilizando la consola y olvidarnos del proceso.
  Con docker run tambien se puede hacer de forma "deatached" (BACKGROUND), agregando " -d " al comando run :" docker run -p 3000:80 -d <img_id>"

- **docker attach <CONTAINER_NAME>**: con este comando podes ATACHARTE A UN PROCESO PARA VER LOS OUTPUTS
- **docker logs <CONTAINER_NAME>**: con este comando podemos ver los logs anteriores de este container

- ###**utilizar consola**: Cuando tenemos un codigo que necesitemos levantar en consola, tenemos 2 formas de utilizarlo
- **docker run -i <CONTAINER_NAME>**, :interactive " UTILIZA EL STDIN aunq no este atachada la terminal "
- **docker run -t <CONTAINER_NAME>**:allocate a pseudo-tty (crea una terminal para comunicarse )
  > Si combinamos el it, nos permite ingresar datos y a su vez permitir que quede abierta la sesion del terminal
  > El comando quedaria **docker run -i -t <CONTAINER_NAME>** o **docker run -it <CONTAINER_NAME>**(utilizando los 2 en un solo comando)
- **docker run --rm <CONTAINER_NAME>**: automatically remove the container when stop
- **docker run --name <my_name>** : crear tags para un container, asi podemos buscarlo mas facil en el docker ps

- **docker run --name <my_name> goals:latest** para especificar una imagen a la cual le creamos el nombre (repository:tag)

  A su vez, para cuando queremos levantar un container que ya corrió. no son los mismo parametros mostrados anteriormente

  > **docker start -a <CONTAINER_NAME>**: Que es lo mismo que vimos anteriormente para (atacharse a un container)
  > **docker start -i <CONTAINER_NAME>**: para visualizar la ventana interactiva
  > **docker start -a -i <CONTAINER_NAME>**

- **docker stop <CONTAINER_NAME>**: para un container que esta levantado
- **docker rm <CONTAINER_NAME>**: borra un container
- **docker rm <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME>**: borra varios containers(separados por un espacio)
- **docker container prune**: borra todos los containers que esten parados

- **EJEMPLO Repaso: (Ejemplo 1, seccion 2)**
  docker run -p 3000:80 -d --rm f1974cfde44f
  En este ejemplo podemos ver que: > -p : se expone el puerto 3000 sobre el 80 para poder acceder > -d : (detached mode) para que nos permita poder seguir utilizando la consola > --rm : para asegurarnos que se borre el container una vez que se pare el container

- **docker cp <path_carpeta> <container_name>:<path_destination>**: copiar los archivos de una carpeta a un contenedor ya creado
  > <path_carpeta> : Puede ser dummy/text.txt o dummy/.(para todos los archivos dentro de esa carpeta)
  > <container_name>: nombre del container corriendo para donde se van a meter esos archivos
  > <path_destination>: el folder donde va a ir a parar lo que se quiera llevar, por ej : /test<- Este path se crea si no existe anteriormente

## IMAGES

- **docker images**: lista todas las imagenes que estan en la pc
- **docker rmi <IMAGE_NAME>**: borrar imagen
- **docker images inspect <image_id>** para ver las configuraciones que se hicieron en la imagen
- **docker prune -a** para borrar todas las imagenes que estan descargadas en el sistema

- **docker build -t goals:latest .** : crear tags para una imagen, para esto se usan 2 partes, name:tag,
  > name o "repositorio": se puede crear un grupo de posibles imagenes
  > tag: mas especial por ahi mostrar diferentes versiones y utilizar la capacidad de tener distintas imagenes de distintos codigos
  > por ejemplo: docker build -t goals:latest .
  > En esta imagen el -t indica el tag, goals : repositorio y latest :el tag

## PUSHING IMAGES

- **docker push <Image_name>**: Para subir al hub de docker nuestras imagenes y poder compartirlas
- **docker pull <Image_name>**: Para obtener desde el hub de docker las imagenes que nos compartieron

  ejemplos:
  docker run -p 3000:3000 --name nodeApp --rm 30729232ebfc

# Docker volumes:

nos ayuda con la persistencia de data y nos ayuda con los problemas de que nos borre la informacion de los containers
conecta una carpeta en nuestra maquina y puede ser utilizada por la pc, pero esta, hace que la data que se creo en ese volume, quede persistente en la pc del host

//EN EL DOCKER FILE:
VOLUME [ "/app/feedback" ]

- HAY 2 TIPOS DE VOLUMES:

  > Anonymus volumes que viven cuanto el contenedor esta activo, si borramos el contenedor el volumen tambien se borra. estos los usamos para que viva en un scope especifico pero si borramos el container o le ponsemos el --rm, al pararlo se borrara
  > Named volumes, estos viven en el host machine , estos van a servir para parar el contenedor pero. van a seguir viviendo
  > bind mounts: Son los que nos bindean un path de unuestra pc a la carpeta (basicamente el nombre del volume es nuestro path de desarrollo)

- Crear un Anonymous volume :

  > docker run -v /app/data
  > Se crea un volume junto a un container (vive mientras este viva) sobrevive si se para el container y se vuelve a levantar pero no mientras se borre con un --rm
  > Este al borrarse cuando se borra un container, no sirve para compartir data entre distintos containers y no se puede reutilizar para guardar infomacion para cuadno se quiere parar y levantar un container .
  > Son buenos para guardar informacion como el nodemodules dentro de un node o guardar informacion para que no sea sobreescrita por otro modulo

- Crear un named volume :

  > docker run -v data:/app/data
  > no pueden ser creados en el dockerfile sino que se crean cuando se cera el container con el -v.
  > se crean en general, sobreviven a que se apague el container y vuelva a crearse.
  > Se pueden utilizar para compartir informacion entre containers.
  > Se pueden Utilizar para guardar informacion para containers y estos se pueden borrar y volver a levantarse mientras q se llame igual se puede recuperar esta informacion.
  > La contra de estos es que no sabemos donde estan guardados en nuestro sistema, sino que docker se encarga de gestionarlos

- Crear un Anonymous volume :

  > docker run -v /path/to/code:/app/data
  > Sabemos su lugar de ubicacion. y no estan atachados a un container sino que pueden utilizarse para varios containers
  > Se pueden utilizar para compartir informacion entre containers. y pueden sobrevivir a restart o removal container
  > Se pueden utilizar para compartir informacion entre containers.
  > Se pueden Utilizar para guardar informacion para containers y estos se pueden borrar y volver a levantarse mientras q se llame igual se puede recuperar esta informacion.

- **Crear un named Volume**:
  > docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
  - feedback es el nombre del volume, y /app/feedback el
- **Removing Anonymous Volumes**
  We saw, that anonymous volumes are removed automatically, when a container is removed.
  This happens when you start / run a container with the --rm option.
  If you start a container without that option, the anonymous volume would NOT be removed, even if you remove the container (with docker rm ...).
  Still, if you then re-create and re-run the container (i.e. you run docker run ... again), a new anonymous volume will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).
  Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or docker volume prune.

> https://headsigned.com/posts/mounting-docker-volumes-with-docker-toolbox-for-windows/

## bind mounts:

    > Como desarrolladores seteamos el path donde vamos aguardar "algo"
      Se pone otro "volume" que el scope sea, el proyecto y que vaya a parar al path especificado de la carpeta (workdir) del proyecto
      "docker run -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "C:/Cursos/docker/Docker/Seccion3/43/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app" feedback-node:volumes"

      docker run -d -p 3000:80 --name, -v folder:pathFolderInsideContainer -v LocationPath:workdir

      Bind Mounts - Shortcuts
        Just a quick note: If you don't always want to copy and use the full path, you can use these shortcuts:

        macOS / Linux: -v $(pwd):/app

        Windows: -v "%cd%":/app

        I don't use them in the lectures, since I want to show an approach that works for everyone (and I don't want to switch between both all the time), but you can use these shortcuts depending on which OS you are working on to save some typing.


     Ejemplo de ambiente de desarrollo utilizando bind mounts:
     docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "C:/Cursos/docker/Docker/Seccion3/43/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app" -v /app/node_modules feedback-node:volumes

     Ejemplo de ambiente de desarrollo utilizando bind mounts pero utilizando el parametro :ro para que sea readonly:
     docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "C:/Cursos/docker/Docker/Seccion3/43/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules feedback-node:volumes
      En este ejemplo existe un problema ya que la carpeta /app/temp y /app/feedback necesitan permisos para modificarse desde la app.
      para esto debemos especificar un volume para dichas carpetas y que estas no esten dentro del ReadOnly proporcionado ya que
      los permisos y los permisos que se van dando van de highorder a lessOrder, para esto tenemos que especificar un volume para dicha carpeta
      y que no este comprendido en el volume que tenga el ro, digamoslos asi, mientras mas largo el path del volume tiene mas importancia
       por ende creamos otro con este :
       docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "C:/Cursos/docker/Docker/Seccion3/43/data-volumes-01-starting-setup/data-volumes-01-starting-setup:/app:ro" -v /app/temp  -v /app/node_modules feedback-node:volumes

    Environments:

      Para esto tenemos que usar el "PORT nRO" en el dockerfile y agregar el $PORT en el environment que se va a hacer export

## Docker Networks:

- para acceder a la ip de nuestra maquina(localhost) desde dentro de un container debemos usar "host.docker.internal"
- docker network create <Name> : Se crea una network para que los containers puedan comunicarse entre si
- docker network ls : Lista todas las netwokrs quehay corriendo


## Test Seccion5:

  - para acceder a la ip de nuestra maquina(localhost) desde dentro de un container debemos usar "host.docker.internal"
