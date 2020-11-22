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



    ejemplos:
    docker run -p 3000:3000 --name nodeApp --rm 30729232ebfc