## Docker-2-Docker-Communication
## The Task is to create two docker container and make them able to communicate with each other.

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

