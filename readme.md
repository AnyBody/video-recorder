# Video Recorder Class

This anyscript template class will allow you to easily create videos from your models. 

It can be included in any model, to produce videos with a single operation.

The camera can be configured to be both statics or a moving camera. 

## Simple Example

```c++
// Include the Camera template class
#include "CameraClassTemplate.any"

Main = {

    VideoCamera MyCam (UP_DIRECTION = y) = {
      Analysis = {
         AnyOperation &ref = Main.MyStudy.InverseDynamics; 
       };
      LookAtPoint = Main.MyModel.Femur.r-{-0.3,0.10,0};   
    };

};

``` 

## Detailed use:

To come.
