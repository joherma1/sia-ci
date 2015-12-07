# How to run a jenkins container

##### 1. Build jenkins arm version
```` docker build -t joherma1/rpi-jenkins . ````
  * Or pull the image if was previously uploaded
``` docker login ```
``` docker push joherma1/rpi-jenkins ```
``` docker pull joherma1/rpi-jenkins ```


##### 2. Config container
  * Create a container without running it
  * Create a volume in the default path (docker inspect cont)
  * Use the docker image jenkins to save space
  * Never run (/bin/true)

```docker create -v /var/jenkins_home --name jenkins_data resin/armv7hf-debian /bin/true```



##### 3. Run jenkins container with the volume

```docker run -p 8080:8080 --volumes-from jenkins_data --name jenkins_sia -d joherma1/rpi-jenkins```


###### 4a. Backup
  * Run another container an link
  * tar the folder jenkins data in pwd

```
docker run --volumes-from jenkins_data -v $(pwd):/backup ubuntu tar cvf  /backup/backup.tar /var/jenkins_home
```


  * Or simply inspect the element and go to the folder
``` docker inspect jenkins_data ```
``` cd /var/lib/docker/volumes/446ebd09b499c236c427fc83a9a33206172f2e9b89662f35bb76296d1cbeb720/_data ```


###### 4b- Restore
  * Another way to create the container and not run it (run without -d)

```
docker run -v /var/jenkins_home --name jenkins_data2 ubuntu /bin/bash
docker run --rm --volumes-from jenkins_data2 -v $(pwd):/backup ubuntu tar xvf /backup/backup.tar -C /var/jenkins_home
```


##### 5- Configure Master Slave
  * Host server (user, java)
    * `sudo adduser jenkins`
    * `sudo usermod -aG docker jenkins`
    * `sudo mkdir /home/jenkins/slave`
    * `sudo chown jenkins:jenkins /home/jenkins/slave`
    * `apt-get install openjdk-8-jdk`
  * Manage Jenkins -> Manage Nodes -> New node -> Dumb Slave
  * Define a label for the group of nodes (docker)
  * Enter the Docker host IP
    * `/sbin/ip route `
    * `172.17.0.1`
    * ` /sbin/ip route|aw '/default/ { print $3 }' `

##### 6- Configure Linux Dash
  * Install node arm
    * `wget https://nodejs.org/dist/v4.2.3/node-v4.2.3-linux-armv7l.tar.gz`
    * `tar -xvf node-v4.2.3-linux-armv7l.tar.gz `
    * `ln -s /opt/node/node-v4.2.3-linux-armv7l/bin/node /usr/local/bin/node`
    * `ln -s /opt/node/node-v4.2.3-linux-armv7l/bin/npm /usr/local/bin/npm`
  * Download Linux Dash
    * https://github.com/afaqurk/linux-dash/wiki/Install-Linux-Dash
    * `node install`
    * `node server/`
  * Install forever to run in background
    * `npm install forever -g`
    * `ln -s /opt/node/node-v4.2.3-linux-armv7l/bin/forever /usr/local/bin/forever`
    * `forever start server/index.js`
    * `forever list`




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

###### Docker cheats
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

