**Docker Compose** is a tool that simplifies the management of multi-container Docker applications. It allows you to define and run multiple containers using a single YAML file (docker-compose.yml), streamlining the orchestration of services like web servers, databases, and caches. With a single command, you can build, start, and manage your entire application stack.

## Docker-based multi-container application setup on a cloud server (EC2)


### Step 1: Create ec2 server, Connect, install docker, check status, start docker. (Below commands are for Amazon Linux AMI)
```
sudo apt update
sudo apt install docker.io
docker --version 
sudo systemctl start docker
systemctl status docker
```
> Note : command to install docker for **Ubuntu** : sudo apt install docker.io

<img width="1198" height="284" alt="image" src="https://github.com/user-attachments/assets/841f7138-71d6-47ed-8012-47348e40552f" />
<img width="889" height="188" alt="image" src="https://github.com/user-attachments/assets/0c94e6a9-443b-44c2-85af-c61446c35b40" />


### Step 2: Import Docker compose dependencies and make it excecutable:
```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose version
```
<img width="1317" height="243" alt="image" src="https://github.com/user-attachments/assets/0bd0eaf8-4993-4cb5-9ead-a268d4b1f844" />


### Step 3: create directory and put content :
```
mkdir testdirectory
cd testdirectory
touch docker-compose.yml
vi docker-compose.yml
```
<img width="721" height="105" alt="image" src="https://github.com/user-attachments/assets/ef798622-f595-4b5e-995f-97d25a7e2bce" />


### Step 4: insert this content :
**By using this code it creates 3 containers**

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

### Step 5: Enter this cmd to run docker compose, check docker images and check logs:
```
sudo docker-compose up -d
sudo docker images
sudo docker-compose logs -f alpine
```
<img width="1721" height="524" alt="image" src="https://github.com/user-attachments/assets/5af8dcce-e582-4bff-98c2-24d57d033ab5" />
<img width="760" height="106" alt="image" src="https://github.com/user-attachments/assets/86a4b377-293c-47e6-bb54-3a1762527861" />


### Step 6: Change inbound rule and access with IP and port number:
security group >> inbound >> all traffic >> anywhere 0.0.0.0

<img width="1780" height="456" alt="image" src="https://github.com/user-attachments/assets/5526b032-84b5-4ee9-a7d7-5ab5798a4bf8" />


EG: http://<your-ec2-public-ip>:8081
    http://<your-ec2-public-ip>:8082

<img width="1168" height="339" alt="image" src="https://github.com/user-attachments/assets/966ca3ec-be98-4fa3-a38e-1e2b5e45f3d5" />
<img width="764" height="157" alt="image" src="https://github.com/user-attachments/assets/9bd01862-0d0f-4f84-9b4c-1541373b0a3e" />



## Step 7: To stop and remove docker and docker-compose:
```
docker-compose down
docker ps
```
<img width="1734" height="149" alt="image" src="https://github.com/user-attachments/assets/3e5e4079-4363-4197-a2fe-79106956f8b0" />


At the end You have successfully defined and run multi-container Docker applications using a single YAML file (docker-compose.yml)

