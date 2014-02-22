---
layout: page
comments: false
sharing: trued
---

## Scene Entry and Exit Discovery
*All of this work is presented in several publications found [here]({{ root_url }}/about.html)*

My research in school focused developing a method to reliably learn scene entry and exit locations in video. More generally, given a camera that is viewing an area (scene), the goal is to automatically learn areas in the camera view that objects (e.g., people, cars, etc.) enter into the scene and exit out of the scene. These entry and exit "zones" can correspond to anything from a doorway to the edge of the camera view. This local camera view work was then extended to camera viewspaces, and various applications of the detected entry/exit regions were explored. My research project consisted of several large pieces. Each is detailed below.

<hr>
#### **Part 1** Learning Entities
As with most scene understanding algorithms, this work uses tracking data as input. Strong tracking algorithms (ones that track objects reliably as they move through the scene) do not work well in busy urban environments, and can be computationally expensive. To compensate for these issues a "weak" tracking algorithm is used instead. Rather than attempting to track entire objects as strong trackers do, weak trackers track salient feature points (e.g., areas of high texture or corner points). Such trackers work well in crowded scenes, though rather than producing a single track as an object moves through the scene, a weak tracker will produce a set of short and frequently broken "tracklets". These weak tracks are then clustered into "entities" using a modified version of mean-shift clustering on the frame level track observations (see the paper for more details). This transforms the set of weak tracks into a set of "entity" tracks (see below). 

<img width="550px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/kltToEntity.jpg"/>

<hr>
#### **Part 2** Learning Entries and Exits
Using the new set of entity tracks, the entity entry and entity exit observations are clustered using mean-shift clustering, and a convex-hull area reduction technique is used to obtain tighter clusters and a more generalized cluster shape (see paper). This results in a set of potential entry and exit regions. These potential regions are then scored using two behavorial reliability metrics - directional consistency, and interaction consistency. Directional consistency constrains the regions to be somewhat directional, and enforces that objects leave and entry or enter an exit in a semi-directional manner (e.g., objects don't spawn in the scene and leave/enter an entry/exit in all directions). Interaction consistency evaluates how other entity tracks in the scene intersect each potential entry and exit region. This enforces the idea that if an entry exists with objects leaving the entry in some direction, other objects in the scene should not intersect the region in the same direction that objects enter into the scene from the entry (the entry/exit region should not be a "through state"). An example of how the described reliability metrics can be used to reduce a set of potential entry/exit regions to reliable entry/exit regions is shown below. 

<img width="500px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/kltStartToEntityStart.jpg"/>

<img width="500px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/potentialToReliableEntries.jpg"/>

In the above reliable entry image, the arrows denote the most popular angle that objects entered into the scene from each entry. Additional results are shown below. 

<img width="500px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/additionalResults.jpg"/>

<hr>
#### **Part 3** Camera Viewspace Extension
The approach described above applies to a single camera view. However, pan-tilt-zoom cameras are commonly used in surveillance applications. These cameras can pan 360 degrees and tilt 90 degrees, allowing them to move around to view different parts of the world. For a given pan-tilt orientation, a fixed view is observed (such as the camera views from Parts 1 and 2). The entire viewspace of a pan-tilt-zoom camera can be captured as a lower hemisphere, shown below:

<img width="300px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/panoinbowl.jpg"/>
<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/panoinbowlpano.jpg"/>

The first image shows the lower hemisphere camera viewspace. The second image shows a projection of the camera viewspace onto the equator plane. This projection is called a spherical panorama, and is used to help visualize the camera viewspace in a concise two dimensional image. Recall that at any given time, the camera is oriented in a particular pan-tilt orientation, and thus is viewing a sub-portion of the overall viewspace. The below images show a local view highlighted in the greater pan-tilt-zoom camera viewspace. 

<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/viewspacepatch3.jpg"/>
<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/local_view.jpg"/>

The entry/exit detection method described in Parts 1 and 2 was extended to work in camera viewspaces using spherical geometry analogs to the local view Euclidean approach. As above, tracking data was collected. This tracking data was is an aggregate of different local view trajectories projected into the viewspace. The entity tracks for several local views are shown below.

<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/global_entities.jpg"/>

As with the local view approach, these trajectories were transformed into a set of potential entry/exit regions, and the scoring mechanism was applied to produce a final set of reliable entry/exit regions, shown below.

<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/camera1_entries.jpg"/>
<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/camera1_exits.jpg"/>

<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/camera2_entries.jpg"/>
<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/camera2_exits.jpg"/>

<hr>
#### **Part 4** Using the Detected Regions
Once regions are learned, they can be used to model occluded paths, and usage statistics can be collected to detect anomalous activities. Example occluded paths that were able to be detected are display below (path starting locations in red, and re-appearance locations in green):

<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/scene1Occlusions.jpg"/>
<img width="250px" src="{{ root_url }}/images/masters_work/scene_entry_exit_discovery/scene2Occlusions.jpg"/>

