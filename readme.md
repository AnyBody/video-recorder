# Video Recorder Class

This anyscript template class will allow you to easily create videos from your models. 

It can be included in any model, to produce videos with a single operation.

The camera can be configured to be both statics or a moving camera. 

## Simple Example

```c++
// Include the Camera template class
#include "Path/to/Video_Camera/CameraClassTemplate.any"

Main = {

  VideoLookAtCamera  MyCam (UP_DIRECTION = y) = 
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
       
       // Determines the speed of the video. Setting it to 
       // nStep/(tEnd-tStart) make the video run in real time. 
       InputFrameRate = 10;
       
       // The operations which should be included in the video.
       Analysis = {
           AnyOperation &ref = Main.MyStudy.Kinematics;
       };
  };

};

``` 

## Creating the video:

To create a video simply select and run the `Create_Video` operation. That will run the model, collect the frames, convert them to a video and finally play the video.

![image](https://cloud.githubusercontent.com/assets/1038978/15822983/f6fd6b8e-2bf8-11e6-88a4-f8d080f815e5.png)

The video will by default be saved together with the main file of the model. This can of course be customized as well. 



## Detailed use:

To come.
