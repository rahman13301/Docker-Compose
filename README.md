Docker Compose is a tool that simplifies the management of multi-container Docker applications. It allows you to define and run multiple containers using a single YAML file (docker-compose.yml), streamlining the orchestration of services like web servers, databases, and caches. With a single command, you can build, start, and manage your entire application stack.

**Docker-based multi-container application setup on a cloud server (EC2)
**

Set up EC2


Installed Docker & Docker Compose


Created 3 containers using a YAML file


Verified logs


Accessed them from the internet


Cleaned up afterward


Step 1: Create ec2 – server – connect – install docker – check status – start docker


Step 2:
1. **sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64" -o /usr/local/bin/docker-compose**
2. **sudo chmod +x /usr/local/bin/docker-compose**
3. **sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose**
4. **docker-compose version**


step 3: create directory :
**mkdir testdirectory**


step 4: change to the directory :
**cd testdirectory**


step 5: create yaml file :
**touch docker-compose.yml**


step 6: put some yaml file content inside this file :
**vi docker-compose.yml**


step 7: insert this content :
by using this code it creates 3 containers


**version: '3.8'
services: nginx: image: nginx:latest container_name: nginx_server ports: - "8081:80"
apache: image: httpd:latest container_name: apache_server ports: - "8082:80"
alpine: image: alpine:latest container_name: alpine_box command: ["sh", "-c", "while true; do echo Hello from Alpine!; sleep 5; done"]**
----------- -----------------
Once inserted code save and quit by using esc+ :wq


Step 8: enter this cmd to run docker compose :
**docker-compose up -d**


Step 9: enter cmd: **docker images**


Step 10: enter cmd: **docker-compose logs -f alpine**
( this cmd checks the logs for particular container )


step 11:
security group >> inbound >> all traffic >> anywhere 0.0.0.0


step 12: copy pub ip >> paste in chrome and add port number
EG: http://<your-ec2-public-ip>:8081


step 13: check for apache
enter same ip and but port no is : 8082


step 14: to stop and remove everything
**docker-compose down**


step 15: **docker ps**
At the end You have successfully,

Set up EC2

Installed Docker & Docker Compose

Created 3 containers using a YAML file

Verified logs

Accessed them from the internet

Cleaned up afterward

defined and run multi-container Docker applications using a single YAML file (docker-compose.yml)
