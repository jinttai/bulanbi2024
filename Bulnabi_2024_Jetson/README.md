# Requirements

1. Install v4l2_camera

   ```
   sudo apt update
   sudo apt install ros-${ROS_DISTRO}-v4l2-camera
   ```
<br/>

2. Change the path of local yolov5

   Download YOLOv5: https://github.com/ultralytics/yolov5

   modify 'Bulnabi_2024_Jetson/yolo_detection/yolo_detection/yolo_detector.py'

   '/home/chaewon/yolov5' -> path of 'yolov5' in your computer
   
   ```
    # define model path and load the model
        model_path = os.path.join(os.getcwd(), 'src/yolo_detection/config/best.pt')
        self.model = torch.hub.load('/home/chaewon/yolov5', 'custom', path=model_path, source='local')
   ```
<br/>

3. (Optional) v4l2-ctl: change v4l2_camera settings

   ```
   sudo apt install v4l-utils

   v4l2-ctl -d /dev/video0 --list-formats-ex   # camera formats 확인
   v4l2-ctl -d /dev/video0 --set-fmt-video=width=640,height=480   # change settings
   ```

<br/>
<br/>


# Build

```
colcon build --symlink-install --packages-select my_bboxes_msg       // cbp my_bboxes_msg
colcon build --symlink-install                                       // cba
source ./install/local_setup.bash
```

<br/>
<br/>


# Run

```
# run each nodes saperately
   ros2 run v4l2_camera v4l2_camera_node
   ros2 run yolo_detection yolo_detector
   ros2 run vehicle_controller controller

# run yolo & v4l2
   ros2 launch yolo_detection yolo_detector.launch.py

# run vehicle_controller & yolo & v4l2
   ros2 launch vehicle_controller vehicle_controller.launch.py
```

<br/>
<br/>
