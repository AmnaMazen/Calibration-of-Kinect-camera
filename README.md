# Calibration-of-Kinect-camera
This repository includes a step-by-step tutorial to calibrate the Kinect camera

This work is done with the help of my students, Chenghao Lin and Naian Tao.

We followed this GitHub repository while implementing some changes

https://github.com/code-iai/iai_kinect2/tree/master/kinect2_calibration#calibrating-the-kinect-one


## Recommended preparation:

1. Print your calibration pattern (for example, we used chess7x9x0.025, which you will find attached to this repository ) and glue it to a flat object. The calibration pattern must be very flat. Also, check with a caliper that the distance between the features of the printed pattern is correct. Sometimes, printers scale the document, and the calibration won't work. For the mentioned pattern, the distance between intersections of black and white corners should be 2.5cm exactly. chess7x9x0.025.pdf

2. Get two tripods, one for holding the calibration pattern and another for holding the
kinect2 sensor. Ideally, the tripod for the kinect2 will have a ball head, which allows you to move it easily and lock it in place before you take an image. The sensor must be stable (and the image is clear and not blurred) before you take an image. The tripod will help you ensure that the sensor does not move between the moment the IR and the RGB images are taken.

4. When recording images for all the steps indicated below (RGB, IR, SYNC), start the recording program and press the space bar to record each image. The calibration pattern should be detected (indicated by color lines overlaid on it), and the image should be clear and stable.
   
5. It is recommended to take images that show the calibration pattern in all areas of the image, and with different orientations of the pattern (tilting the pattern relative to the plane of the image), and at least two distances. So you can easily reach 100 images per calibration set.

6. We normally start at a short distance, where the calibration pattern covers most of the image. There, we take several pictures tilting the calibration pattern vertically and then horizontally. Imagine a ray coming out of the camera sensor. This makes sure that you have images where the calibration pattern is not perpendicular to that ray.

7. Then we move the calibration pattern further away, and for different orientations (tilting) of the pattern, we take many images so that we calibration pattern is present around most of the camera image. For example, at first the calibration pattern is on the left upper corner. Then on the next image on the upper middle, then on the upper right corner. Then some images where the calibration pattern is in the middle vertically, etc...

   
## Detailed steps:

1. If you haven't already, start the kinect2_bridge with a low number of frames per second (to make it easy on your CPU):
   $ rosrun kinect2_bridge kinect2_bridge _fps_limit:=2
   
2. create a directory for your calibration data files, for example "kinect_cal_data"
    
$ mkdir ~/kinect_cal_data

$ cd ~/kinect_cal_data

3.  Record images for the color camera:

$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 record color

 Click the image window and push the "s" button to save the current image for calibration.
 
4. Calibrate the intrinsics:

$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 calibrate color

This creates a file called "calib_color.yaml"
   
5. Record images for the IR camera:

$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 record ir
   
6. Calibrate the intrinsics of the IR camera:
   
$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 calibrate ir

This creates a file called "calib_ir.yaml"
   
7. Record images on both cameras synchronized:

$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 record sync

8. Calibrate the extrinsics:
   
$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 calibrate sync

This creates a file called "calib_pose.yaml"

9.  Calibrate the depth measurements:
    
$ rosrun kinect2_calibration kinect2_calibration chess7x9x0.025 calibrate depth

This creates a file called "calib_depth.yaml"
 
10. Find out the serial number of your kinect2 by looking at the first lines printed out by the kinect2_bridge. The line looks like this: device serial: 008716665247.

11. Create the calibration results directory in kinect2_bridge/data/$serial:
    
$ roscd kinect2_bridge/data

$ mkdir 008716665247

12. Copy the following files from your calibration directory (~/kinect_cal_data) into the directory you just created:
    
    calib_color.yaml,  calib_depth.yaml,  calib_ir.yaml, and calib_pose.yaml
    
13. Restart the kinect2_bridge and be amazed at the better data.
