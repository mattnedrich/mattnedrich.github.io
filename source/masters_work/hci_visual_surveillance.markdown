---
layout: page
comments: false
sharing: trued
---

## Human Computer Interaction for Visual Surveillance
*All of this work is presented in a technical report found [here]({{ root_url }}/about.html)*

Part of my Masters work involved designing and developing an interface to help control and manage large distributed camera networks. Such large networks may consist of hundreds if not thousands of static and active (pan-tilt-zoom) cameras, and may become cumbersome to use as the size of the network grows. Furthermore, such networks may only be managed by a few operators at a time. This usually requires the network operators to have intimate knowledge of the camera topology (where the cameras are placed in the world), in order to respond quickly to events. The system I worked on attempts to integrate many cameras into a single user interface, allowing for multiple levels of camera control within the network, and is easily extensible for modular processes which may be applied to a camera or across many cameras in the network (e.g., object tracking). 

The system was built on top of [NASA Worldwind](http://worldwind.arc.nasa.gov/java/), an Java open source alternative to Google Earth. One of the ideas behind building it on top of NASA Worldwind was that it made the system capable of supporting many cameras across large distances, and provided a geographic framework which all of the cameras may be registered to. Cameras are integrated by providing connection, control, video, and registration information. Users can connect to any integrated camera, control the camera, and stream video (shown below) 

<img width="500px" src="{{ root_url }}/images/masters_work/hci/video_ss5_1.jpg"/>

The system allows users to have different "levels of control" when controlling a pan-tilt-zoom camera (not including standard joystick control). One way to control the camera is through the live video window. Clicking anywhere on the video stream will re-position the camera to focus on the location clicked. Users can also click anywhere on the map. When doing so, the system finds the closest camera that can view the ground location of interest, opens up a video stream to that camera, and orients the camera to view the selected location. This approach enables operators to view any location in the world without worrying about where the cameras exist.

In between the local "click on camera video feed" and global "click on the map" levels of control, the system also offers a middle level of control. Pan-tilt-zoom cameras have the capability of moving around to view different parts of the world, though at any given time they are only viewing a small subset of the total area they can view. By moving the camera to numerous overlapping pan-tilt-zoom orientations, and collecting an image at each location, the images may be stitched together to create a spherical panorama (shown below), capturing the full view-space of the camera. 

<img width="300px" src="{{ root_url }}/images/masters_work/hci/pano.jpg"/>

The system leverages such spherical panoramas by allowing users to drag a viewbox around the panorama in order to quickly orient the camera to any desired view. 

<img width="500px" src="{{ root_url }}/images/masters_work/hci/video_ss5_2.jpg"/>

In addition to interacting with the distributed camera network (controlling, streaming video, etc.) the system allows the integration of other modules. One such module is object tracking. The user can open a video stream to a camera, and invoke a remote tracking module (on a different machine) that starts to track objects in the cameras view. Tracking results are then sent back to the interface to be displayed and saved. Below are two screenshots of such a module. The tracker being used is a modified version of the KLT (Kanade Lucas Tomasi tracker). Such trackers track good feature points on objects over time (thus a single object may generate multiple tracks as seen below). 

<img width="500px" src="{{ root_url }}/images/masters_work/hci/video_ss5_3.jpg"/>

<img width="500px" src="{{ root_url }}/images/masters_work/hci/video_ss5_4.jpg"/>


