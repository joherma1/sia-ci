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
  docker tag -f joherma1/rpi-sia:$COMMIT joherma1/rpi-sia:$TAG
  #Also as the latest
  docker tag -f joherma1/rpi-sia:$COMMIT joherma1/rpi-sia:latest
  
  #Push the image and the tag
  docker push joherma1/rpi-sia:latest
  docker push joherma1/rpi-sia:$TAG
  ```