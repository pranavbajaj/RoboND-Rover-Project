## Project: Search and Sample Return
---

[image1]: ./output_images/original.jpg
[image2]: ./output_images/path.jpg
[image3]: ./output_images/obstacles.jpg
[image4]: ./output_images/original_rock_img.jpg
[image5]: ./output_images/id_rock.jpg 
[video1]: ./output/test_mapping.mp4


### Notebook Analysis

#### 1. For obstacle identification I have used 'color_thresh' function. Attributes to this funciton are image, mask , lower rgb threshold and upper rgb threshold. First path was identified with help of cv2.inRange(image, lower, upper). Then I have used cv2.bitwise_not function to find the remaining area in the image. But this remaining area also contain the unvisible part. So I have used mask to clear out the unidentified part and the remaining part is obstacles.                    **Note: I have predefined the lower rgb threshold to [160, 160, 160] and upper to [255,255,255]**
#### Original
![alt text][image1]
#### Path
![alt text][image2]
#### Obstacles
![alt text][image3]

#### 2. For rock identification I have used 'rock_thresh' function. It is similar to above function, only changes are in threshold values. Lower = [100,100,0] and Upper = [255,255,50]
#### Original 
![alt text][image4]
#### Thresholded
![alt text][image5]

#### 3.In process_image() following setps are taken:
##### A. 'perspect_transform()' function is applied on incoiming image. Output of this function are an perspective transformed image and a mask. Mask here is the area of the optained img which is not visible. This help us to differentiate between obstacles and not visible area.
##### B. Then with 'color_thresh()' and 'rock_thresh()' function we get three image, identifing path, obstacles and rock respectively.
##### C. First this three image are transfered to rover co-ordinates and then to world co-ordinates with help of 'rover_coords()' and 'pix_to_world()' function. Output of 'pix_to_world()' is the array of co-ordinates specifing each prixel with respect to world co-ordinates 
##### D. All the obstacles co-ordinates  is set red, Path is set blue and Yellow rock is set to white in world map.
##### I have done one more thing, I have removed all the red pixels (obstacles co-ordinates) from the world map which overlaps with the Blue pixels (Path). This give us a more clean world map.
![Alt text for your video][video1]

---
### Autonomous Navigation and Mapping

#### 1 In`perception_step()` I have done some addition changes in 'process_image()' which differs from the function used in Notebook. When ever rover sees a yellow rock, it will store rocks co-ordinates in Rover.rock_world_position and distance to rock from rover in Rover.rock_dist.

#### 2. In `decision_step()` I have modified rover to behave  4 differt mode - 'forward', 'avoid_obstacles', 'go_to_rock' and 'avoide_dead_end'
##### A. In 'forward' mode, rover find the path in front of it and follows it. If while moving forward it find yellow rock, it will switch to 'go_to_rock' mode. If obstacles comes in front of rock it switch to 'avoid_obstacles' mode. If rover come to dead end, it will switch to 'avoide_dead_end' mode.
##### B. In 'avoide_obstacles' rover will stop first, then it will look for more clear path to its right or left. Deppending upon the clear path to its left or right, steering value will be set and rover will avoide obstacle.
##### C. In 'go_to_rock' mode, rover will first come to rest, then steer so that it can face towards rock and then it will start moving towards rock. Once it gets near rock, it will stop again and rock will be picked.
##### D. 'avoide_obstacles' mode is inefficient to avoid dead ends, to I have added 'avoide_dead_end' mode.

#### 3. Launching in autonomous mode your rover can navigate and map autonomously.  
**Note: Resolution -  1280 X 720 , Graphics quality - Good, frames per second - 20 to 24.** 

---
#### 4. Areas where my code can fail
##### 1. When rover locates a yellow rock and then while moving toward rock, an obstacles comes in between.
##### 2. When rover is moving toward a yellow rock and need to fallow a very curve mountain boundary 
##### 3. Rover cannot avoide obstacles which are in fornt of its tires but out of camera range  [In real life we can use different sensors for that].

---

