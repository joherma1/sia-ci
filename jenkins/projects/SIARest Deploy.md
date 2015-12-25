### Build parametrized
  * String Parameter
    * Name: **TAG**
    * Default Value: **latest**
    
### Build
  * Execute shell   
  ```
    #Pre-requisite
    #Database up and running
    docker stop siarest
    docker rm siarest_bak
    docker rename siarest siarest_bak
    
    docker run --device=/dev/ttyACM0:/dev/cu.usbmodem1411 -p 3000:3000 --name siarest --link mongo_sia:mongo -d joherma1/rpi-siarest:$TAG
  ```