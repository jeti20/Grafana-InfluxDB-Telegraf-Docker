# TelegrafAgent-InfluxDB-Grafana
A project showing how to create an example monitoring using EC2 instances on AWS and Telegraf Agent. On Docker.

My suggestion of the architecture

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/c36d7ac2-15c8-4557-ac0f-e69d69d130c8)



Start with creating EC2 Instance with Ubuntu. On purpose of this project I used the free tier options. Set up security group.

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/c6b5dba7-a09e-40a9-b7d6-5cc50051218f)


Port 22(SSH) and 3000 (Grafana) and 8086 (InfluxDB) should be accessable.

Go to place where u store your private key for ssh and connect with this machine. On AWS website go into your instance, find and click "Connect" button, then switch to "SSH client" and copy example which involve your EC2 instance public IP. Open console in place where you store privatekey for ssh (or use PuTTY) paste copied example with yourip address. If you fail to connect try with "sudo"

If you established connection with your machine type

<br>**sudo apt-get update**
<br>**sudo apt install docker.io -y**
<br>**sudo apt install docker-compose**
<br>**sudo service docker start**
<br>**sudo systemctl status docke**r - checking if docker is running
<br>**sudo usermod -aG docker ubuntu** - Addind ubuntu user do Docker group so I can execute command without sudo all the time. Now if u write for example "docker info" or "docker --version" you still will get "permission denied...". (not sure if there will be this issue with docker --version)

You need to reset your connection with the server.

Now We have to perform "Port binding" in docker container

![image](https://github.com/jeti20/TelegrafAgent-InfluxDB-Grafana/assets/61649661/905aa51a-5408-4695-8829-cb27c7cd5452)

<br>**docker network create monitor_network** -> creating custom network called "monitor_network"
<br>**docker network ls** -> checking if there is new network with our name

If we wish to have persisting data once we bring down our container, we need to specify the docker volumes using volume option for InfluxDB & Grafana container. We can gice any name to the volumes. Let's name them influxdb-volume and grafna volume respectively.

<br>**sudo docker volume create influxdb-volume**
<br>**sudo docker volume create grafana-volume**
<br>**docker volume ls**

Now we need to create docker-compose.yml
 
<br>**vim docker-compose.yml** -> check file "docker-compose.yaml" in tihs repository
<br>**docker-compose up -d** -> running containers
<br>**docker ps -a** -> checking all containers

Here when I wanted to check my Grafana and InfluxDB in browser 

<br>Grafana: http://Public_IPv4_address:3000
<br>InfluxDB: http://Public_IPv4_address:8086

Create account after u enter InflxuDB on port 8086. Create bucket and organization in my case i created them both called "a". After you register window with TOKEN should pops out. Copy this token somhere for later use.

I cannot connect for like a 1h. Everything indicates that everything its ok but still cannot access them. So in security group I deleted all inbound rules and added them one more time, first choosing my ip and then choosing 0.0.0.0/0... it works now, not sure why.

<Installing Telegraf
<br>**sudo apt install telegraf**

Configure Telegraf so he send data to InfluxDB

<br>**cd /etc/telegraf**
<br>**vim telegraf.config**

Go down and find OUTPUT PLUGINS and uncomment this tow lines and put public IP from AWS in the url . Paste also TOKEN which you copied before from your website. also enter the bucket and organization name.

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/04623c32-0720-4c3e-bd94-aacd8e3b6602)

also check if by default there is no Prometheus ON, if is put # to comment it.

<br>**sudo service telegraf start**
<br>**sudo journalctl -f -u telegraf.service**

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/8e9d2d9b-b9eb-498d-8f7d-beaf01a0f1b0)


After conecting telegraf with influxdb you can choose metrics to visualate in inflxudb.

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/9dc81755-06f8-4e34-928d-d23804ca3e3c)

Go into your grafana webiste

http://Public_IPv4_address:3000

And add InfluxDB as a source

![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/8c3341be-0256-45ce-91a9-4a3d0a1b5e25)


![image](https://github.com/jeti20/Docker-InfluxDB-Grafana.Telegraf/assets/61649661/cf815026-3e55-4e9f-b86a-739be5176db7)




