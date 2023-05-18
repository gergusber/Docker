Step by step:
1:
docker run --name mongodb --rm -p 27017:27017 -d mongo
To run a mongodb and expose the port 27017 that is the port that is defined

2:
we create the docker file and we use the "mongodb://host.docker.internal:27017/course-goals", 
if u are using linux you should use your internal ip. instead of <host.docker.internal>
to connect to our "localhost"
and run : to create the image

- docker build -t goals-node . => To create the image of this file
- docker run --name goals-backend --rm -d -p 80:80 goals-node = > To create the container
  Remember to use -p to map the port 80 to the 80 to my machine

3: The react app:

Create the dockerfile for frontend(keep in mind that the port is 3000)
Run the container

Build your image :

- docker build -t goals-react .
- docker run --name goals-frontend --rm -p 3000:3000 -it goals-react // quick note here, do not forget about the ** -it **  bc it wont work 


now, we will set them into a network:
for that, first create the network:


- docker network create goals-net

now we run again the mongodb, this time without port due is running in the 27017 in local network. and specify network tag.

- docker run --name mongodb --rm -d --network goals-net mongo

now its time for backend server.
we need to update the port of the "localhost of the network "
- docker build -t goals-node . ( to update the path to "mongodb://mongodb:27017/course-goals")
- docker run --name goals-backend --rm -d -p 80:80 --network goals-net goals-node

now the frontend:
 we need to update the backend url that is published in the port 80.
- docker build -t goals-react .
- docker run --name goals-frontend --rm -p 3000:3000 -it goals-react 


for mongo persistance 
we stop the mongodb: > docker stop mongodb
and we assign a volume 
- docker run --name mongodb --rm -d -v data:/data/db --network goals-net mongo

but we also want to add username and password to the mongodb :
MONGO_INITDB_ROOT_USERNAME, MONGO_INITDB_ROOT_PASSWORD

- docker run --name mongodb --rm -d -v data:/data/db --network goals-net -e MONGO_INITDB_ROOT_USERNAME=german -e MONGO_INITDB_ROOT_PASSWORD=secret mongo

so after this we need to update the connection string in app.js of backend
docker run --name goals-backend --rm -d -p 80:80 --network goals-net goals-node.


now, we generate development server onbackned with volumes to update it.
3 volumes:
1rst one for the development server
2nd for the logs folder
3rd one for the nodemodules.

docker run --name goals-backend -v /home/german/cursos/Docker/Seccion5-Multicontainer/backend:/app -v logs:/app/logs -v /app/node_modules --rm -d -p 80:80 --network goals-net goals-node


now we update the frontend to can update it during development process

- docker run --name goals-frontend -v /home/german/cursos/Docker/Seccion5-Multicontainer/frontend/src:/app --rm -p 3000:3000 -it goals-react
- docker run --name goals-frontend --rm -p 3000:3000 -it goals-react

