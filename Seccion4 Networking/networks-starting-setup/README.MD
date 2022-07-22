TO PLAY WITH THIS PART FIRST WE RUN THIS DOCKER


$ docker build -t favorites-node .
$ docker run --name favorites -p 3000:3000 -d --rm favorites-node
    -d = Detached mode
    --rm = remove when its stoped 
    favorites-node = image(The one that we created)
    --name = name of the container
    -p = port


-- from the container, we can comunicate with the web, the thing its when we try to comunicate to another container. 
if we dont have the server of the mongo db.. we need to create a container with mongodb

If we are trying to run the app with a server, the mongodb://mongodb:27017/swfavorites url , must change to tell docker that its looking another port:
in this case mongodb://**mongodb**:27017/swfavorites => mongodb://**host.docker.internal**:27017/swfavorites

---

-- CREATE A NETWORK SO YOU CAN COMUNICATE WITH ANOTHER CONTAINERS.

docker run --network <name_network> 
docker will manage the ip for each 
we need to create a network 

$ docker network create <Name> 
run: docker network create favorites-net 

then run your docker images
docker run --name mongodb -d  --network favorites-net favorites-node
docker run --name favorites -p 3000:3000 -d --rm  --network favorites-net favorites-node
