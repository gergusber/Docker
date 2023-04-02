first we add mongodb image :

docker run --name mongodb --rm -d -p 3006:27017 mongo

- with a name
- -rm remove after finish
- -d need the console for continue updating
- -p expose port 27017 from image to the local 3006.

in backend we build our image:
  > docker build -t goals-node
and we run it : with 
> docker run -name goals-backend --rm -d goals-node