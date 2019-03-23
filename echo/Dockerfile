FROM node:6

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/
# Bundle app source
COPY laravel-echo-server.json /usr/src/app/laravel-echo-server.json

RUN npm install

EXPOSE 3000
CMD [ "npm", "start" ]
