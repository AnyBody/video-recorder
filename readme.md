# Video Recorder Class

This anyscript `class_template` will allow you to easily create videos from your
models.

It can be added into any model to produce videos with a single click. It can
also create animated Gif files.

The camera can be configured as a fixed camera or as a moving camera to follow
the certain parts of the model.

## Usage

Here is a quick example for the impatient:

```c++
// Include the Camera template class (Somewhere before Main)
#include "Path/to/Video_Camera/CameraClassTemplate.any"

Main = 
{

  VideoLookAtCamera  MyCam (UP_DIRECTION=y, CREATE_GIF=1) = 
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
       BackgroundColor = {1, 1, 1};
       
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

### Previewing the camera

The class does not record what what is in the Model View. Instead the camara
view is configured programatically. Class members like `CameraLookAtPoint`,
`CameraDirection`, and `CameraFieldOfView` define what the camera records. This
is usefull since it allows you get consistent videos.

To get a preview of what the camera records, select and run the `Preview`
operation. It will collect a single frame and launch the Windows image viewer.

![image](https://cloud.githubusercontent.com/assets/1038978/24335121/950633c8-1277-11e7-8920-e17bc15ba4d3.png)


Once you are satisfied with the camera settings of the class you are ready create a full video.


### Creating the video

To create a video simply select and run the `Create_Video` operation. That will
run the model, collect the frames, convert them to a video and finally play the
video.

![image](https://cloud.githubusercontent.com/assets/1038978/24335125/a289d55e-1277-11e7-8075-8f823af024f5.png)

The video will by default be saved together with the main file of the model.
This can of course be customized as well.


### More examples

Two examples are included here: 

1. [Fast toy model to play with the settings](https://github.com/AnyBody/video-recorder/blob/restructure/ExampleModel.any)
2. [A full human model with a camera that spins around the subject](https://github.com/AnyBody/video-recorder/blob/restructure/ExampleModelHuman.any)


## Class description:

### Syntax: 
```
  VideoLookAtCamera <object name> (UP_DIRECTION = y, <arg>=<value>) = 
  {
    <member name> = <value>;
  }
```

### Template arguments
| Argument      | Default  | Description         |
| --------------|:--------:|-------------------| 
|  UP_DIRECTION |  y       | The up direction of the camera. Can be x/y/z |
|  CREATE_GIF   |   0      | Set to 1 to always create animated Gif files from the vide.  |
|  \_OVER_WRITE |   1      | If the camra is allow to overwrite existing videos. On by default. | 
|  _DEBUG       |   0      | Set to 1 to allow debugging the class. This will show the output of ffmpeg.|
| \_CLEAN\_UP_IMAGES |  1  | If images shoud be deleted after the video is created. Set to 0 to keep the image frames. | 


### Expected members:
| Member name        |     Type     | Description         |
| -------------------|:------------:|-------------------| 
|  CameraLookAtPoint |   AnyVec3    | The point the camera focus on. This can be fixed point or some moving point on the model. |
|  CameraFieldOfView |   AnyFloat   | The vertical field of view in meters at the LookAtPoint. | 
|  CameraDirection   |     AnyVec3  |  The direction which the camera is placed (In global coordinate with respect to the LookAtPoint). |



### Optional members: 

The  following class members are optional. They are listed in order of usefulnes.

| Member name     |    Default   |     Type     | Description         |
| ----------------|:-----------:|:------------:|-------------------| 
| BackgroundColor |    {1,1,1}   | AnyVec3 | The background color ({red, green, blue}) used for the video. Defaults to white. |
| Counter         | -            | AnyInt |  Counter for numbering the saved images. This defaults to the camera class builtin counter. 
| VideoName       |    "Cam1"    |   AnyString  | File name of the video that will be created |
| VideoResolution | {1920, 1080} |  AnyIntArray | Resolution of the output video in {Hight,Width}   |
| VideoInputScale |     1        |   AnyFloat   | The ratio between video resolution and input images saved from anybody. Default is to save images in same resolution as the output video. It is an advantage to set this to 2 or 4 when making videos with a low resolution |
| VideoInputFrameRate | 30       | AnyInt       | Determines the speed of the video. Setting it to `nStep/(tEnd-tStart)` make the video run in real time. |
| GifResolution   | {600, 600}   | AnyIntArray  | Resolution of the animated Gif file. Note: Setting this will not alter the orginal aspect ratio. | 
| VideoCodec      |    "libxvid" |  AnyString   | The video codec ffmepg will use to create the video. Choose "libxvid" for best for compatibility (eg. with PowerPoint) or "libx264" for best performance |
| VideoBitRate    |  8000        | AnyInt       | Video BitRate in KiloByte |
| VideoStartFrame |   0          |  AnyInt      | This is the start frame used when creating Videos. This can be used to for skipping some of the initial frames.   |
| VideoPathFFMPEG | "ffmpeg.exe" |  AnyString   | The path to the ffmpeg binary. Defaults to the same directory as the template class. |
| VideoOutputPath | Dependent    | AnyString | The directory to save the video in. This defaults to the directory of the current main file if not specified. |
| CameraDistance  | 10           |   AnyFloat   | The distance from the camera to the scene. This does NOT determine the ize of scene, since Perspective is off by default. To zoom in/out, use `CameraFeildOfView`. | 
| CameraUpDirection | Dependent  | AnyVec3      | The updirection of the camera as a vector. This is set by default by the UP_DIRECTION but can be overwritten.  |
