# Video Recorder Class

This anyscript template class will allow you to easily create videos from your models. 

It can be included in any model, to produce videos with a single operation.

The camera can be configured to be both statics or a moving camera. 

## Simple Example

```c++
// Include the Camera template class
#include "Path/to/Video_Camera/CameraClassTemplate.any"

Main = {

  VideoCamera MyCam (UP_DIRECTION = y) = 
  {
       // The point the camera focus on (Can be a moving point)
       LookAtPoint = Main.MyModel.Femur.Knee.r + {0, -0.1, 0};  
       
       // The vertical field of view in meters
       VerticalFieldOfView = 1;
       
       // The direction which the camera is placed
       // (In global coordinate with respect to the LookAtPoint. Can also be a moving vector)
       LookPoint2Camera_direction = {1, 1, -1};
       
       // The background color used for the video
       BackgroundColor = {1,1,1};
       
       // The operations which should be included in the video.
       Analysis = {
           AnyOperation &ref = Main.MyStudy.InverseDynamics;
       };
};

};

``` 

## Detailed use:

To come.
