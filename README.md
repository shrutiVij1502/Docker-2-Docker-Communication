## Docker-2-Docker-Communication
## Task 1 - Create two docker container and make them able to communicate with each other.
## Task 2 - Create two docker container, one of which should be of mongoDB and the application should connected to the mongoDB container.

# TASK 1 
## Create two docker container and make them able to communicate with each other.

### Step 1 - Installation of Docker 

``` For windows - https://runnable.com/docker/install-docker-on-windows-10 ```

### Step 2 - Here, we are taking NGINX Image to work on, so to pull the Docker image for nginx, we use the Command - 

``` docker pull nginx:alpine ```

### Step 3 - Containers are designed to be isolated. But they can send and receive requests to other applications, using networking.
For example: a web server container might expose a port, so that it can receive requests on port 80. Or an application container might make a connection to a database container.

After that, Check that the bridge network is running: You can check it’s running by typing 

``` docker network ls ```

To create your own bridge network , we need to run this command - 

<img width="323" alt="image" src="https://user-images.githubusercontent.com/67600604/199723632-63124d9a-167a-45e3-b3e3-65aa3e371168.png">

then, Create two container with the same network so that they can communicate with each other 

<img width="409" alt="image" src="https://user-images.githubusercontent.com/67600604/199724789-2f195476-8d40-4fc3-8ea9-10751eb05399.png">

like this 

```
docker run -d -p 8080:80 --network="shruti-network" --name server1 nginx:alpine
docker run -d -p 8081:80 --network="shruti-network" --name server2 nginx:alpine
```

We can check the specification of container using the command 

``` docker inspect server2 ```

<img width="671" alt="image" src="https://user-images.githubusercontent.com/67600604/199726017-57418613-3eb7-44b4-aeed-70ef9888024e.png">

Now we have created two containers of nginx on the same network, so they should communicate with each other, for this we will try to ping one container to the other, so for that we will take shell of server2 and will try to ping server1 from the server2

``` docker exec -it server2 sh ```

<img width="302" alt="image" src="https://user-images.githubusercontent.com/67600604/199726143-5e9ad080-32a4-49aa-8dd6-f9829e805221.png">

server2 is succesfully able to ping the server1 - Hence they are able to communicate with each other

# TASK 2
## Create two docker container, one of which should be of mongoDB and the application should connected to the mongoDB container.

### Step 1 - Run a Mongo DB Docker Container

``` docker run -d --name my-mongo mongo:latest ```

### Step 2 - Run a PHP container 

``` docker run -d -p 8020:80 --name php-apache php:5-apache ```

Note: This will run a php container, but in order to be able to connect to mongo db container, you need to link this container to mongo db container. So, we will create the Linked container like this - 

``` docker run -d -p 8020:80 --link my-mongo --name php-mongo-test php:5-apache ```

Now, you should be able to see two container running by typing: ```docker ps``` command.

## Step 3 - Install Mongo Php connector

``` docker exec -it PHP_IMAGE_ID bash ```

Now, you are into the shell terminal of php container. You will need to install php-mongo dependencies.

Run following commands:

```
apt-get update
apt-get install openssl libssl-dev libcurl4-openssl-dev
pecl install mongo
echo "extension=mongo.so" > /usr/local/etc/php/conf.d/mongo.ini
```

Note - In above steps, we basically installed few dependencies required for mongo db connector, and installed mongo db php extension, and included that in php.ini list, and then restart the container using the ```docker start Container-ID``` and ```docker stop Container-ID```

Now, Prepare a phpfile in /var/www/html directory say info.php, and put following content:

```
<?php
print phpinfo();

On your browser, try: localhost:8082/info.php
```

Now, Run Php code that connects to Mongo DB - I named the File as mogo.php


```
<?php
$connection = new MongoClient( "mongodb://my-mongo:27017" );

$collection = $connection->selectCollection('db-name', 'collection-name');
if (!$collection) {
        echo 'not connected to collection';
        exit;
}

echo "MongoDB is successfully connected to the PHP container";

Test is using - http://localhost:8020/mongo.php
```



