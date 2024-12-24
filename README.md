## demo app - developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

#
#

### With Docker

##### Create a network

Step: Create docker network

```bash
docker network create mongo-network 
```

#

##### Creating MongoDB container
Step: start mongodb 

```bash
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --net mongo-network --name mongodb mongo    
```

#

##### Creating MongoDBExpress container to manage MongoDB graphically
Step 1: start mongo-express

```bash
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express:1.0.0-alpha
```

Step 2: open mongo-express from browser

```bash
http://host-ip:8081
```

Step 3: create `user-accounts` and `my-db` databases in mongo-express. _(graphically you do this)_

#

##### Creating node(backend) container to establish connection with mongodb database

Before building image, you need to make changes in some files.

```bash
sudo vi index.html
```

const response = await fetch('http://**192.168.194.52**:3000/get-profile');

const response = await fetch('http://**192.168.194.52**:3000/update-profile',

**Change these ip values with your host machine ip in your index.html file when you clone this repo**


Step 1: Create docker image for nodeJS backend

Go to 3-Tier-NodeJS-MongoDBManagement-MongoDB location

```bash
cd 3-Tier-NodeJS-MongoDBManagement-MongoDB
```

Create docker image

```bash
docker build . -t sample-node-app
```

Step 2: Deploy to container

```bash
docker run -d -p 3000:3000 --name sample-node-app --net mongo-network sample-node-app:latest
```

#

### Optional
##### Starting application locally _(Don't use this method, deploy with container)_
Start your nodejs application locally - go to `app` directory of project 

```bash
cd 3-Tier-NodeJS-MongoDBManagement-MongoDB/app
npm install 
node server.js
```
    
Access you nodejs application UI from browser

```bash
http://host-ip:3000
```

#
#

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

```bash
docker-compose -f docker-compose.yaml up
```
