### Git
  * Url   
  ```
  https://github.com/joherma1/siarest.git
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
  #Build Database
  docker build -t joherma1/rpi-mongo repo/deploy/mongo/.
  
  #Run database
  docker run --name mongo_sia_test -d joherma1/rpi-mongo
  
  #Build docker image
  docker build -t joherma1/rpi-siarest:$GIT_COMMIT repo/.
  
  #Test docker image
  docker run -i --link mongo_sia_test:mongo --rm joherma1/rpi-siarest:$GIT_COMMIT /bin/bash -c 'cd /opt/siarest/; npm test'
  ```
  
### Post Build Actions
  * Task   
  ```
  docker stop mongo_sia_test
  docker rm -v mongo_sia_test
  ```