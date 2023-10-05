# mine-and-forest-radar-dataset
This repository contains links and instructions for a radar odometry dataset associated with the _Do we need scan-matching in radar odometry?_ paper submitted to the ICRA 2024 conference.

## Download

The file format is ROS1 _.bag_ files with all sensory data as recorded during the _Forest_ and _Mine_ experiments. The bag files are split into 5GB parts to make the download and handling easier. 

The files are available under this link:  [Download from ORU.se](https://cloud.oru.se/s/rHbRad83A764nmx)

## Topics

The dataset provides sensor measurements from these sensors:

 - Sensrad Hugin A3-Sample (solid-state 4D radar)
 - Ouster OS1-32 (3D lidar)
 - Xsens MTi-30 (IMU)
 - Emlid Reach RS2+ (RTK-GNSS receiver pair, Forest experiment only)

in these topics:

    /emlid_gnss/fix (sensor_msgs/NavSatFix)       
    /emlid_gnss/nmea_sentence (nmea_msgs/Sentence)          
    /emlid_gnss/post_rtk_fix (sensor_msgs/NavSatFix)       
    /emlid_gnss/time_reference (sensor_msgs/TimeReference)   
    /hugin_raf_1/radar_data (sensor_msgs/PointCloud2)     
    /imu/data (sensor_msgs/Imu)             
    /imu/mag  (geometry_msgs/Vector3Stamped)
    /imu/time_ref (sensor_msgs/TimeReference)   
    /ouster/points (sensor_msgs/PointCloud2)     
    /tf (tf2_msgs/TFMessage)           

### TF and IMU

The _/tf_ topic contains all static transforms between the sensors, except for the IMU, which is located behind the Hugin sensor close to the _base_link_ frame origin. From space reasons, the IMU is rotated 90 degrees clockwise around its Z axis, such that the _X_imu_ axis points to the right, the _Y_imu_ axis points forward and the _Z_imu_ axis points up. The IMU internal filter in the IMU is set to the _VRU General  profile_ which handles the magnetometer readings as a source of yaw drift suppression, however the heading is not absolutely referenced by it.

### Hugin radar

The Hugin radar is set to the _Short range_ profile, which assures the highest range resolution for the price of 50m range. It produces between 5,000-10,000 points that include the power and Doppler velocity values. The radar messages are timestamped in the sensor, however they arrive with approx 0.1s lag.

### OS1-32 lidar

The lidar scans are recorded directly as PointCloud2 messages in the bag files to make the re-use easier. The lidar is PTP-synchronized with the recording computer.

### Reach RS2+ GNSS reference

In the Forest experiment, a pair of the Reach RTK-GNSS receivers was used to obtain a positioning reference. The RTK solution was computed offline and inserted into the bag files as the _/emlid_gnss/post_rtk_fix_ topic. The original singe-fix solution is stored in the _/emlid_gnss/fix_ topic. The clock of the recording computer is not synchronized with the GNSS clock, there may be some offset between those two. The ROS message stamps are based on the time of the NMEA message arrival over the serial link.
 
## User feedback

This dataset is a part of a work in progress on a radar SLAM solution for harsh environments. We welcome suggestions regarding this dataset or its documentation in the _Issues_ section of this repository.
