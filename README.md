# Docker:

-  **docker images ls** : lista las imagenes
-  **docker ps -a** : lista los containers 
-  **docker ps **: lista todos los containers corriendo
-  **docker run <image> **: levanta una imagen del hub y la corre localmente 
- ** docker build** : crea una imagen a partir de un dockerfile
-  **docker stop <nombre-proceso>**: para la ejecucion del container
-  **docker run -p 3000:80 <img_id>** : levanta el container en el puerto 80, el puerto donde esta publicando el servicio .
este caso es: docker run -p <Puerto_app>:<Puerto_pc>