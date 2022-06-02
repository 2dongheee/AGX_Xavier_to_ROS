# AGX_xavier2ROS


NVIDIA Xavier AGX was used. 
Jetpack's jetsion_multimedia_api was utilized and Modified "camera_v4l2_cuda" and "jpeg_code" to implement code that sends compressed images in ROS messages.
#
## Prerequisites

Jetpack SDK Components must be installed.
(including GPU computing, multimedia, graphics, and a  library set for accelerating computer vision...)

* JetPack SDK  
* ROS
#
## Usage
  > $ roslaunch agx_cam2ros agx.launch
#
## Result
* Real-time check
![test_multicam](./Image/test_multicam)

* Camera image from ROS msg
![test_multicam2](./Image/test_multicam2)
#