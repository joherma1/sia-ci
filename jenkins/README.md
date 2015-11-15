# How to run a jenkins container
##### 1. Config container
  * Create a container without running it
  * Create a volume in the default path (docker inspect cont)
  * Use the docker image jenkins to save space
  * Never run (/bin/true)

```docker create -v /var/jenkins_home --name jenkins_data jenkins /bin/true```

##### 2. Run jenkins container with the volume

```docker run -p 8080:8080 --volumes-from jenkins_data --name jenkins.sia -d jenkins```

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
