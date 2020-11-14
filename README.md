# Docker:

-  **docker images ls** : lista las imagenes
-  **docker ps -a** : lista los containers 
-  **docker ps**: lista todos los containers corriendo
-  **docker run <image>**: levanta una imagen del hub y la corre localmente 
-  **docker build** : crea una imagen a partir de un dockerfile
-  **docker stop <nombre-Container>**: para la ejecucion del container
-  **docker start <nombre-Container>**: levanta nuevamente un docker que corto la ejecucion, podemos verlo en docker ps -a
-  **docker run -p 3000:80 <img_id>** : Crea un nuevo container a partir de una imagen en el puerto 80, el puerto donde esta publicando el servicio .
este caso es: docker run -p <Puerto_app>:<Puerto_pc>

-  **Docker run vs Docker start**: docker run ( crea un nuevo container ) setea la consola en modo atached, esto nos permite hacer pruebas, logs y demas, esto es porque "attached" significa que esta escuchando al output del container
  con docker start, el proceso queda trabajando en background. por lo que este nos permite seguir utilizando la consola y olvidarnos del proceso.
  Con docker run tambien se puede hacer de forma "deatached" (BACKGROUND), agregando " -d " al comando run :" docker run -p 3000:80 -d <img_id>"

-  **docker attach CONTAINER_NAME**: con este comando podes ATACHARTE A UN PROCESO PARA VER LOS OUTPUTS 
-  **docker logs CONTAINER_NAME**: con este comando podemos ver los logs anteriores de este container 