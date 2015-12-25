### Build parametrized
  * String Parameter
    * Name: **TAG**
    * Default Value: **latest**
    
### Build
  * Execute shell   
  ```
  #Pre-requisite
  #Database up and running
  docker stop sia
  docker rm sia_bak
  docker rename sia sia_bak
  
  docker run -p 8888:8080 -e POSTGRES_PASSWORD=agricultura.1 -e POSTGRES_USER=sia -e POSTGRES_SCHEMA=sia --name sia --link postgres-sia:postgres -d joherma1/rpi-sia:$TAG
  ```