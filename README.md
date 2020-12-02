# Docker:

-  **docker images ls** : lista las imagenes
-  **docker ps -a** : lista los containers 
-  **docker ps**: lista todos los containers corriendo
-  **docker run <image>**: levanta una imagen del hub y la corre localmente 
-  **docker build** : crea una imagen a partir de un dockerfile
-  **docker stop <CONTAINER_NAME>**: para la ejecucion del container
-  **docker start <CONTAINER_NAME>**: levanta nuevamente un docker que corto la ejecucion, podemos verlo en docker ps -a
-  **docker run -p 3000:80 <img_id>** : Crea un nuevo container a partir de una imagen en el puerto 80, el puerto donde esta publicando el servicio .
este caso es: docker run -p <Puerto_app>:<Puerto_pc>

-  **Docker run vs Docker start**: docker run ( crea un nuevo container ) setea la consola en modo atached, esto nos permite hacer pruebas, logs y demas, esto es porque "attached" significa que esta escuchando al output del container
  con docker start, el proceso queda trabajando en background. por lo que este nos permite seguir utilizando la consola y olvidarnos del proceso.
  Con docker run tambien se puede hacer de forma "deatached" (BACKGROUND), agregando " -d " al comando run :" docker run -p 3000:80 -d <img_id>"

-  **docker attach <CONTAINER_NAME>**: con este comando podes ATACHARTE A UN PROCESO PARA VER LOS OUTPUTS 
-  **docker logs <CONTAINER_NAME>**: con este comando podemos ver los logs anteriores de este container 

- ###**utilizar consola**: Cuando tenemos un codigo que necesitemos levantar en consola, tenemos 2 formas de utilizarlo
- **docker run -i <CONTAINER_NAME>**, :interactive " UTILIZA EL STDIN aunq no este atachada la terminal "
- **docker run -t <CONTAINER_NAME>**:allocate a pseudo-tty (crea una terminal para comunicarse )
    >Si combinamos el it, nos permite ingresar datos y a su vez permitir que quede abierta la sesion del terminal
    >El comando quedaria **docker run -i -t <CONTAINER_NAME>** o **docker run -it <CONTAINER_NAME>**(utilizando los 2 en un solo comando)
- **docker run --rm <CONTAINER_NAME>**: automatically remove the container when stop
   
- **docker run --name <my_name>** : crear tags para un container, asi podemos buscarlo mas facil en el docker ps 

- **docker run --name <my_name> goals:latest** para especificar una imagen a la cual le creamos el nombre (repository:tag) 


  A su vez, para cuando queremos levantar un container que ya corrió. no son los mismo parametros mostrados anteriormente
  >**docker start -a <CONTAINER_NAME>**: Que es lo mismo que vimos anteriormente para (atacharse a un container)
  >**docker start -i <CONTAINER_NAME>**: para visualizar la ventana interactiva 
  >**docker start -a -i <CONTAINER_NAME>**

- **docker stop <CONTAINER_NAME>**: para un container que esta levantado
- **docker rm <CONTAINER_NAME>**: borra un container 
- **docker rm <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME> <CONTAINER_NAME>**: borra varios containers(separados por un espacio) 
- **docker container prune**: borra todos los containers que esten parados 


- **EJEMPLO Repaso: (Ejemplo 1, seccion 2)**
  docker run -p 3000:80 -d --rm f1974cfde44f
   En este ejemplo podemos ver que:
      > -p : se expone el puerto 3000 sobre el 80 para poder acceder
      > -d : (detached mode) para que nos permita poder seguir utilizando la consola
      > --rm : para asegurarnos que se borre el container una vez que se pare el container


- **docker cp <path_carpeta> <container_name>:<path_destination>**: copiar los archivos de una carpeta a un contenedor ya creado
  > <path_carpeta> : Puede ser dummy/text.txt o dummy/.(para todos los archivos dentro de esa carpeta)
  > <container_name>: nombre del container corriendo para donde se van a meter esos archivos
  > <path_destination>: el folder donde va a ir a parar lo que se quiera llevar, por ej : /test<- Este path se crea si no existe anteriormente

## IMAGES

- **docker images**: lista todas las  imagenes que estan en la pc
- **docker rmi <IMAGE_NAME>**: borrar imagen  
- **docker images inspect <image_id>** para ver las configuraciones que se hicieron en la imagen
- **docker prune -a** para borrar todas las imagenes que estan descargadas en el sistema

- **docker build -t goals:latest .** : crear tags para una imagen, para esto se usan 2 partes, name:tag,
    > name o "repositorio": se puede crear un grupo de posibles imagenes
    > tag: mas especial por ahi mostrar diferentes versiones y utilizar la capacidad de tener distintas imagenes de distintos codigos 
  por ejemplo: docker build -t goals:latest . 
    > En esta imagen el -t indica el tag, goals : repositorio  y latest :el tag


## PUSHING IMAGES
 - **docker push <Image_name>**: Para subir al hub de docker nuestras imagenes y poder compartirlas
 - **docker pull <Image_name>**: Para obtener desde el hub de docker las imagenes que nos compartieron


    ejemplos:
    docker run -p 3000:3000 --name nodeApp --rm 30729232ebfc


## Docker volumes:
nos ayuda con la persistencia de data y nos ayuda con los problemas de que nos borre la informacion de los containers
conecta una carpeta en nuestra maquina y puede ser utilizada por la pc, pero esta, hace que la data que se creo en ese volume, quede persistente en la pc del host

//EN EL DOCKER FILE: 
VOLUME [ "/app/feedback" ]

- HAY 2 TIPOS DE VOLUMES:
 > Anonymus  volumes  que viven cuanto el contenedor esta  activo, si borramos el contenedor el volumen tambien se borra. estos los usamos para que viva en un scope especifico pero si borramos el container o le ponsemos el --rm, al pararlo se borrara
 > Named volumes, estos viven en el host machine , estos van a servir para parar el contenedor pero. van a seguir viviendo 

- **Crear un named Volume**:
  > docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
  - feedback es el nombre del volume, y /app/feedback el
- **Removing Anonymous Volumes**
  We saw, that anonymous volumes are removed automatically, when a container is removed.
  This happens when you start / run a container with the --rm option.
  If you start a container without that option, the anonymous volume would NOT be removed, even if you remove the container (with docker rm ...).
  Still, if you then re-create and re-run the container (i.e. you run docker run ... again), a new anonymous volume will be created. So even though the anonymous volume wasn't removed automatically, it'll also not be helpful because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).
  Now you just start piling up a bunch of unused anonymous volumes - you can clear them via docker volume rm VOL_NAME or docker volume prune.

>https://headsigned.com/posts/mounting-docker-volumes-with-docker-toolbox-for-windows/

