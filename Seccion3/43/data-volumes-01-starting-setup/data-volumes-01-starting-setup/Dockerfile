FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# Se hace esto para que el node_modules quede en un volume y que no se pierda cuando nosotros ponemos la carpeta de nuestro local en un bind mount
# asi este lo actualiza, pero como en nuestra pc no tenemos el  node_modules creado lo borra una vez que hizo el npminstall
# VOLUME [ "/app/node_modules" ]

CMD [ "npm", "start" ]

