FROM node:14



WORKDIR /app


COPY package.json .

RUN npm install

COPY . .

ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

# Se hace esto para que el node_modules quede en un volume y que no se pierda cuando nosotros ponemos la carpeta de nuestro local en un bind mount
# asi este lo actualiza, pero como en nuestra pc no tenemos el  node_modules creado lo borra una vez que hizo el npminstall
# VOLUME [ "/app/node_modules" ]
# C:\Cursos\docker\Docker\Seccion3\data-volumes-01-starting-setup

# Para utilizar el env tag
# docker run -d -p 3000:8000 --env PORT=8000 --rm --name feedback-app -v feedback:/app/feedback -v "C:\Cursos\docker\Docker\Seccion3\data-volumes-01-starting-setup:/app:ro" -v /app/temp  -v /app/node_modules feedback-node:env


CMD [ "npm", "start" ]

