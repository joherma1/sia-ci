Based on the official [dockerfile](https://github.com/docker-library/redmine/blob/master/2.6/Dockerfile)
* [`2.6`, `latest` (Dockerfile)](https://github.com/joherma1/sia-ci/blob/master/redmine/Dockerfile)

![empty](http://cdn.shopify.com/s/files/1/0279/1227/t/5/assets/highsnobiety-logo-badge-white.svg?75070636155751373 "empty")


### How to run a redmine container

##### 1. Build redmine arm version
  * Build  
  ```
  docker build -t joherma1/rpi-redmine .
  ```
  * Or pull the image if was previously uploaded   
  ```
  docker pull joherma1/rpi-redmine
  ```

##### 2. Data container
  * Create a container without running it
  * Create a volume in the default path (docker inspect cont)
  * Use the docker image redmine to save space
  * Never run (/bin/true)   
  ```
 docker create -v /usr/src/redmine/files --name sia-redmine-data resin/armv7hf-debian /bin/true 
  ```


##### 3. Run redmine database (Postgres)
```
docker run -d --name redmine-postgres -e POSTGRES_PASSWORD=enimder -e POSTGRES_USER=redmine joherma1/rpi-postgres
```

##### 3. Run redmine 
```
docker run -d -p 3001:3000 --volumes-from sia-redmine-data --name sia-redmine --link redmine-postgres:postgres joherma1/rpi-redmine
```

##### 4. Configure repository with git
  * Log via ssh and clone the repo into the mounted contatiner
  ```
docker exec -it sia-redmine bash
mkdir /usr/src/redmine/files/repos/
chown redmine:redmine /usr/src/redmine/files/repos/
git clone https://github.com/joherma1/siarest.git /usr/src/redmine/files/repos/siarest
```
  * Create a cron to fetch the repo every 5 minutes
  ```
sudo crontab -e -u redmine
*/5 * * * * cd /usr/src/redmine/files/repos/siarest && git fetch --all
```
  * Create a repo inside the project (http://sia-sysreg.fortiddns.com:3001/projects/sia/repository) and add the git folder
  ```
/usr/src/redmine/files/repos/siarest/.git
```
