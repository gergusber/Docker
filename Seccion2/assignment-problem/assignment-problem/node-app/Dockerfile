FROM node
WORKDIR /myapp

COPY package.json /myapp

RUN npm install

COPY . /myapp

EXPOSE 3000

CMD [ "node","server.js" ]