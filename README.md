# ROS Implementation
The ROS implementation repo for both publisher and subscriber packages


## Access to the System 
There is currently only one user on the system and the password is </br>
<h3><strong><i>175900</i></strong></h3>

## Getting Started 
This project is a starting point for a ROS application.

A few resources to get you started if this is your first ROS project:

* ROS Wiki: ROS Introduction </br>
For help getting started with ROS, view ROS online documentation, which offers tutorials, samples, guidance on mobile development, and a full API reference.

## Running this project
Ensure that the camera is connected to the system then open a terminal to run the following: 
</br>
compile all catkin_ws packages which includes the astra_camera package with
```
cm 
```
now you can run the camera with
```
roslaunch astra_camera astra.launch
```
<strong>It important to note that, the fps for the camera has been reduced to 4fps as against the normal 30fps in order to work with the current dataset. This can be easily changed back.</strong>
## Running Publisher Package
To start the publisher package that listens to messsage on <i>/camera/depths/points</i> topic, open terminal to run the following:</br>

source ROS in this new terminal window with
```
sod
```
now you can run the publisher package with following command in this same terminal.
```
rosrun pfh_publisher pfh_publisher_node
```
<strong>It important to note that, all subscribers nodes needs to be connected to the publisher before it starts publishing. The line that code that ensures that can be commented out if not needed in <strong>HistogramPublisher.cpp</strong> ln 25-26 </br>
```
~/catkin_ws/src/ros_thesis/simulation/pfh_publisher/src/HistogramPublisher.cpp
```

## Running Subscriber Package
To start the current subscriber package that listens to messsage on <i>/pf_histogram</i> topic, open terminal to run the following:</br>
source ROS in this new terminal window with
```
sod
```
now you can run the subscriber package with a launch file that starts all LSTM node using the following command in this same terminal.
```
roslaunch pfh_subscriber_fn pfh_lstm.launch
```
finally to see the aggregated results, use the following command in a new terminal after sourcing.
```
rosrun pfh_subscriber_fn pfh_lstm_aggregator.py
```

## Change LSTM Model 
Just incase there is a need to change the LSTM model, simply follow these steps:
* open the folder where the current model is saved
```
~/catkin_ws/src/ros_thesis/simulation/pfh_subscriber_fn/SavedModel/new
```
* replace the new LSTM model by deleting the old one and save the new with the name <strong><i>"newhandGestureModel.h5"</i></strong>. 
* re-run the lstm nodes with the launch file 

## General Experience

#### Conditional Removal - Range Condition
During mine experiment, after processing,  the points fades away resulting in wrong classification or PFH not been pushlished at all. I had to make changes to the xyz range condition which are declared in <strong>HistogramPublisher.h</strong> ln 83-88 </br>
The file can be found in 
```
~/catkin_ws/src/ros_thesis/simulation/pfh_publisher/include/pfh_publisher/HistogramPublisher.h
```
The initial range set 
```
const float min_X_ = -1.25f;
const float max_X_ = 1.25f;
const float min_Y_ = -1.5f;
const float max_Y_ = 1.5f;
const float min_Z_ = 0.0f;
const float max_Z_ = 0.7f;
```
For my experiment I used
```
const float min_X_ = -2.5f;
const float max_X_ = 2.5f;
const float min_Y_ = -1.75f;
const float max_Y_ = 2.0f;
const float min_Z_ = -0.0f;
const float max_Z_ = 0.7f;
```
