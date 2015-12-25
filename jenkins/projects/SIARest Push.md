### Build parametrized
  * String parameter
    * Name: **COMMIT**
    * Default Value: **latest**
    
  * String Parameter
    * Name: **TAG**
    * Default Value: **latest**
    
### Build
  * Execute shell   
  ```
  #Tag the version 
  docker tag -f joherma1/rpi-siarest:$COMMIT joherma1/rpi-siarest:$TAG
  #Also as the latest
  docker tag -f joherma1/rpi-siarest:$COMMIT joherma1/rpi-siarest:latest
  
  #Push the image and the tag
  docker push joherma1/rpi-siarest:latest
  docker push joherma1/rpi-siarest:$TAG
  ```