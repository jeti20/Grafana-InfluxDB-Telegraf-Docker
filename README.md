# TelegrafAgent-InfluxDB-Grafana
A project showing how to create an example monitoring using EC2 instances on AWS and Telegraf Agent. On Docker.

Start with creating EC2 Instance with Ubuntu. On purpose of this project I used the free tier options. Set up security group.

![image](https://github.com/jeti20/TelegrafAgent-InfluxDB-Grafana/assets/61649661/51ee05c0-4e40-42e1-aacd-e0a181decd0a)

Port 22(SSH) and 3000 (Grafana) and 8086 (InfluxDB) should be accessable.

Go to place where u store your private key for ssh and connect with this machine. On AWS website go into your instance, find and click "Connect" button, then switch to "SSH client" and copy example which involve your EC2 instance public IP. Open console in place where you store privatekey for ssh (or use PuTTY) paste copied example with yourip address. If you fail to connect try with "sudo"

If you established connection with your machine type

<br>**sudo apt-get update**
<br>**sudo apt install docker.io -y**
<br>**sudo apt instal docker-compose**
<br>**sudo service docker start**
<br>**sudo systemctl status docke**r - checking if docker is running
<br>**sudo usermod -aG docker ubuntu** - Addind ubuntu user do Docker group so I can execute command without sudo all the time. Now if u write for example "docker info" or "docker --version" you still will get "permission denied...". (not sure if there will be this issue with docker --version)

You need to reset your connection with the server.

Now We have to perform "Port binding" in docker container

![image](https://github.com/jeti20/TelegrafAgent-InfluxDB-Grafana/assets/61649661/905aa51a-5408-4695-8829-cb27c7cd5452)

<br>**docker network create monitor_network** -> creating custom network called "monitor_network"
<br>**docker network ls** -> checking if there is new network with our name

If we wish to have persisting data once we bring down our container, we need to specify the docker volumes using volume option for InfluxDB & Grafana container. We can gice any name to the volumes. Let's name them influxdb-volume and grafna volume respectively.

<br>**docker volume create influxdb-volume**
<br>**docker volume create grafana-volume**
<br>**docker volume ls**

Now we need to create docker-compose.yml
 
<br>**vim docker-compose.yml** -> check file "docker-compose.yaml" in tihs repository
<br>**docker-compose up -d** -> running containers
<br>**docker ps -a** -> checking all containers

    

Sources for this projects:
https://www.youtube.com/watch?v=3G1RsUgJNg0
https://www.youtube.com/watch?v=Wh5Ub94iseE
https://www.youtube.com/watch?v=5ZE92U47hvg
