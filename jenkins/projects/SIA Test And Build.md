### Git
  * Url   
  ```
  https://github.com/joherma1/sia.git
  ```
  * Branch   
  ```
  */develop
  ```
  * Checkout to a sub-directory   
  ```
  repo
  ```
  
### Build
  * Execute shell   
  ```
  #Build database
  #cd repo/deploy/postgres
  docker build -t joherma1/rpi-postgres repo/deploy/postgres/.
  #cd -
  
  #Run database
  docker run -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia --name postgres_sia_test -d joherma1/rpi-postgres
  
  #Build JDK 
  docker build -t joherma1/rpi-java:7-jdk repo/deploy/java/.
  
  #Build SIA image
  docker build -t joherma1/rpi-sia:$GIT_COMMIT repo/.
  
  #Test SIA image
  docker run -i --link postgres_sia_test:postgres --rm -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia -e POSTGRES_SCHEMA=sia joherma1/rpi-sia:$GIT_COMMIT /bin/bash -c 'cd /opt/sia; mvn exec:java -Dexec.mainClass=org.sysreg.sia.model.InitializeDatabase'
  docker run -i --link postgres_sia_test:postgres --rm -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia -e POSTGRES_SCHEMA=sia joherma1/rpi-sia:$GIT_COMMIT /bin/bash -c 'cd /opt/sia; mvn integration-test'
  ```
  
### Post Build Actions
  * Task   
  ```
  docker stop postgres_sia_test
  docker rm -v postgres_sia_test
  ```