# Video Recorder Class

This anyscript class_template will allow you to easily create videos from your models. 

It can be included in any model, to produce videos with a single click.

The camera can be configured as a fixed camera or as a moving camera to follow the certain parts of the model. 

## Simple Example

So here is quick example for those who wants to get started quickly:

```c++
// Include the Camera template class
#include "Path/to/Video_Camera/CameraClassTemplate.any"

Main = {

  VideoLookAtCamera  MyCam (UP_DIRECTION = y) = 
  {
       // The point the camera focus on.
       CameraLookAtPoint = Main.MyModel.Femur.Knee.r;  
       
       // The vertical field of view in meters
       CameraFieldOfView  = 1;
       
       // The direction in which the camera is placed. In global 
       // coordinate with respect to the LookAtPoint. 
       // (Can also be a time varying vector)
       CameraDirection  = {1, 1, -1};
       
       // The background color used for the video
       BackgroundColor = {1,1,1};
       
       // Determines the speed of the video. Setting it to 
       // nStep/(tEnd-tStart) make the video run in real time. 
       VideoInputFrameRate  = 10;
       
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


## Class description:

### Syntax: 
```
  VideoLookAtCamera <object name> (UP_DIRECTION = y) = 
  {
    <member name> = <value>;
  }
```

### Template arguments
| Argument      | Default  | Description         |
| --------------| ---------|:-------------------:| 
|  UP_DIRECTION |  y       | The up direction of the camera. Can be x/y/z |
|  \_OVER_WRITE |   1      | If the camra is allow to overwrite existing videos. On by default. | 
|  _DEBUG       |   0      | Set to 1 to allow debugging the class. This will show the output of ffmpeg.|
| \_CLEAN\_UP_IMAGES |  1  | If images shoud be deleted after the video is created. Set to 0 to keep the image frames. | 


### Expected members:
| Member name        |     Type     | Description         |
| -------------------| -------------|:-------------------:| 
|  CameraLookAtPoint |   AnyVec3    | The point the camera focus on. This can be fixed point or some moving point on the model. |
|  CameraFieldOfView |   AnyFloat   | The vertical field of view in meters at the LookAtPoint. | 
|  CameraDirection   |   The direction which the camera is placed (In global coordinate with respect to the LookAtPoint). |



### Optional members: 


| Member name     |    Default   |     Type     | Description         |
| ----------------| -------------| -------------|:-------------------:| 
| BackgroundColor |    {1,1,1}   | AnyVec3 | The background color ({red, green, blue}) used for the video. Defaults to white. |
| Counter         | -            | AnyInt |  Counter for numbering the saved images. This defaults to the camera class builtin counter. 
| VideoName       |    "Cam1"    |   AnyString  | File name of the video that will be created |
| VideoResolution | {1920, 1080} |  AnyIntArray | Resolutiono of the output video in {Hight,Width}   |
| VideoInputScale |     1        |   AnyFloat   | The ratio between video resolution and input images saved from anybody. Default is to save images in same resolution as the output video. It is an advantage to set this to 2 or 4 when making videos with a low resolution |
| VideoInputFrameRate | 30       | AnyInt       | Determines the speed of the video. Setting it to `nStep/(tEnd-tStart)` make the video run in real time. |
| VideoCodec      |    "libxvid" |  AnyString   | The video codec ffmepg will use to create the video. Choose "libxvid" for best for compatibility 
(eg. with PowerPoint) or "libx264" for best performance |
| VideoBitRate    |  8000        | AnyInt       | Video BitRate in KiloByte |
| VideoStartFrame |   0          |  AnyInt      | This is the start frame used when creating Videos. This can be used to for skipping some of the initial frames.   |
| VideoPathFFMPEG | "ffmpeg.exe" |  AnyString   | The path to the ffmpeg binary. Defaults to the same directory as the template class. |
| VideoOutputPath | Dependent    | AnyStringVar | The directory to save the video in. This defaults to the directory of the current main file if not specified. |
| CameraDistance  | 10           |   AnyFloat   | The distance from the camera to the scene. This does NOT determine the ize of scene, since Perspective is off by default. To zoom in/out, use `CameraFeildOfView`. | 
| CameraUpDirection | Dependent  | AnyVec3      | The updirection of the camera as a vector. This is set by default by the UP_DIRECTION but can be overwritten.  |

