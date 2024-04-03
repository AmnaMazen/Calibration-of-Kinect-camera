# Calibration-of-Kinect-camera
This repository includes a step-by-step tutorial to calibrate the Kinect camera

Kinect2 Calibration
https://github.com/code-iai/iai_kinect2/tree/master/kinect2_calibration#calibrating-the-kinect-one
Recommended preparation:
Print your calibration pattern (for the examples, we used chess7x9x0.025 ) and glue it to a flat object. It is very important that the calibration pattern is very flat. Also, check with a caliper that the distance between the features of the printed pattern is correct. Sometimes printers scale the document, and the calibration won't work. For the mentioned pattern, the distance between intersections of black and white corners should be 2.5cm exactly. chess7x9x0.025.pdf
Get two tripods, one for holding the calibration pattern, and another one for holding the
kinect2 sensor. Ideally, the tripod for the kinect2 will have a ball head, to allow you to move it easily and lock it in place before you take an image. It is very important that the sensor is stable (and the image is clear and not blurred) before you take an image. The tripod will specially help you to make sure that the sensor has not moved between the moment the IR and the RGB images are taken.
When recording images for all the steps indicated below (RGB, IR, SYNC), start the recording program, then press space bar to record each image. The calibration pattern should be detected (indicated by color lines over layed on the calibration pattern), and the image should be clear and stable.
It is recommended to take images that show the calibration pattern in all areas of the image, and with different orientations of the pattern (tilting the pattern relative to the plane of the image), and at least two distances. So you can easily reach 100 images per calibration set.
We normally start at a short distance, where the calibration pattern covers most of the image, there we take several pictures tilting the calibration pattern vertically, then horizontally. Imagine a ray coming out of the camera sensor, this makes sure that you have images where the calibration pattern is not perpendicular to that ray.
Then we move the calibration pattern further away, and for different orientations (tilting) of the pattern, we take many images so that we calibration pattern is present around most of the camera image. For example, at first the calibration pattern is on the left upper corner. Then on the next image on the upper middle, then on the upper right corner. Then some images where the calibration pattern is in the middle vertically, etc...
Detailed steps:
1. If you haven't already, start the kinect2_bridge with a low number of frames per second (to make it easy on your CPU): rosrun kinect2_bridge kinect2_bridge _fps_limit:=2
2. create a directory for your calibration data files, for example:
3. Record images for the color camera:
. Click the image window and push s button to save the
    
 
    ~/kinect_cal_data
mkdir ~/kinect_cal_data; cd
        current image for calibration.
rosrun kinect2_calibration kinect2_calibration
   chess7x9x0.025 record color
 
4. Calibrate the intrinsics:
5. Record images for the ir camera:
6. Calibrate the intrinsics of the ir camera:
7. Record images on both cameras synchronized: 8. Calibrate the extrinsics:
   rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 calibrate color
   rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 record ir
    rosrun kinect2_calibration
 
kinect2_calibration chess7x9x0.025 calibrate ir
    rosrun kinect2_calibration
 
kinect2_calibration chess7x9x0.025 record sync
    rosrun kinect2_calibration kinect2_calibration
    chess7x9x0.025 calibrate sync
    rosrun kinect2_calibration kinect2_calibration
 
chess7x9x0.025 calibrate depth
 
device serial: 008716665247
   roscd
 
kinect2_bridge/data; mkdir 008716665247
 
calib_color.yaml  calib_depth.yaml  calib_ir.yaml
 
calib_pose.yaml
 
from interbotix_xs_modules.arm import InterbotixManipulatorXS
import rospy
9. Calibrate the depth measurements:
10. Find out the serial number of your kinect2 by looking at the first lines printed out by the kinect2_bridge. The line looks like this:
11. Create the calibration results directory in kinect2_bridge/data/$serial:
12. Copy the following files from your calibration directory (~/kinect_cal_data) into the directory you just created:
13. Restart the kinect2_bridge and be amazed at the better data.
