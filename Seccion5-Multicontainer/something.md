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
