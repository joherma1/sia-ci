# How to run a jenkins container
##### 1. Config container
  * Create a container without running it
  * Create a volume in the default path (docker inspect cont)
  * Use the docker image jenkins to save space
  * Never run (/bin/true)

```docker create -v /var/jenkins_home --name jenkins_data resin/rpi-raspbian /bin/true```

##### 2. Run jenkins container with the volume

```docker run -p 8080:8080 --volumes-from jenkins_data --name jenkins_sia -d joherma1/rpi-jenkins```

###### 2a. Backup
  * Run another container an link
  * tar the folder jenkins data in pwd

```
docker run --volumes-from jenkins_data -v $(pwd):/backup ubuntu tar cvf  /backup/backup.tar /var/jenkins_home
```

###### 2b- Restore
  * Another way to create the container and not run it (run without -d)

```
docker run -v /var/jenkins_home --name jenkins_data2 ubuntu /bin/bash
docker run --rm --volumes-from jenkins_data2 -v $(pwd):/backup ubuntu tar xvf /backup/backup.tar -C /var/jenkins_home
```

##### 3- Configure Master Slave
  * Manage Jenkins -> Manage Nodes -> New node -> Dumb Slave
  * Create a new user with ssh login and docker
    * ``` sudo adduser jenkins ```
    * ```sudo usermod -aG docker jenkins```
  * Create a new folder where jenkins will put the server (remotely)
  * Define a label for the group of nodes
  * Enter the Docker host IP
    * ```17.17.42.1```
    * ``` /sbin/ip route|aw '/default/ { print $3 }' ```

###### Appendix
```
1- Install plugins
	git
	github
	post task
	locale
		en_US

2- Install node
	from tar ball
	modify Path -> Manage-> environment variables -> PATH /var/jenkins_home/node-v0.12.7-linux-x64/bin:$PATH

3- Install mongo
	from tar ball
	modify Path
```

Container
docker create -v /var/jenkins_home --name jenkins_data joherma1/rpi-jenkins /bin/true

docker run -p 8080:8080 --volumes-from jenkins_data --name jenkins.sia -d joherma1/rpi-jenkins

docker build -t joherma1/rpi-jenkins .
docker run -p 8080:8080 joherma1/rpi-jenkins


#Docker cheats
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
#Removing volumes
docker rm -v $(docker ps -a -q)
docker rmi -f $(docker images -q)

curl $(docker-machine ip default):8080
docker logs -f node-sia


#DEBUG from layer with terminal
docker run --rm -it <id_last_working_layer> bash -il
#Run a command in a running container
docker exec -it [container-id] bash

