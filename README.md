**Docker Compose** is a tool that simplifies the management of multi-container Docker applications. It allows you to define and run multiple containers using a single YAML file (docker-compose.yml), streamlining the orchestration of services like web servers, databases, and caches. With a single command, you can build, start, and manage your entire application stack.

# Docker-based multi-container application setup on a cloud server (EC2)


## Step 1: Create ec2 – server – connect – install docker – check status – start docker
```
yum install -y docker
docker --version 
systemctl status docker
systemctl start docker
```

## Step 2: Import Docker compose dependencies and make it excecutable:
```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose version
```


## step 3: create directory and put content :
```
mkdir testdirectory
cd testdirectory
touch docker-compose.yml
vi docker-compose.yml
```

## step 4: insert this content :
by using this code it creates 3 containers

```
version: '3.8'
 
services:
  nginx:
    image: nginx:latest
    container_name: nginx_server
    ports:
      - "8081:80"
 
  apache:
    image: httpd:latest
    container_name: apache_server
    ports:
      - "8082:80"
 
  alpine:
    image: alpine:latest
    container_name: alpine_box
    command: ["sh", "-c", "while true; do echo Hello from Alpine!; sleep 5; done"]
```

----------- -----------------
Save and Quit

## Step 5: enter this cmd to run docker compose :
```
docker-compose up -d
docker images
```

## Step 6: Checking logs of containers:
```
docker-compose logs -f alpine
```

## step 7: Change inbound rule and access with IP and port number
security group >> inbound >> all traffic >> anywhere 0.0.0.0

EG: http://<your-ec2-public-ip>:8081
    http://<your-ec2-public-ip>:8082


## step 8: To stop and remove everything
```
docker-compose down
docker ps
```

At the end You have successfully defined and run multi-container Docker applications using a single YAML file (docker-compose.yml)

