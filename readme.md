![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/8f31ab06-204c-4889-be15-785a8dc779f8)

# The parameters of the Vestek robot 
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/deb5b0df-6aed-4a83-928c-50edebd2b323)

| parameters | value || parameters | value | 
|---|---|---|---|---|
| l1(m) | 0.176 || W1(m) | 0.6 | 
| l2(m) | 0.176 || L1(m) | 0.7 |
| l3(m) | 0.29 || H1(m) | 0.268 |
| l4(m) | 0.451 || W2(m) | 0.25 | 
| l5(m) | 0.495 ||L2(m) | 0.7 |
| l6(m) | 0.12 ||H2(m) | 0.182 |
| l7(m) | 0.055 || W3(m) | 0.5 | 
| l8(m) | 0.02 ||L3(m) | 0.7 |
| 𝜃_𝑏 𝑚𝑎𝑥(deg) | ±14.5° ||H3(m) | 0.7 |

* To make it climb up the stairs, more than 4 wheels should be contact with the flat surface.
  

# How to simulate it in simulink
## Some tips to use simulink
   * Double click the blank and write the name of the box. Or use Library Browser.
   * Group the boxes to make it looks better. Drag or click with <Shift> and <Ctrl+G>.
   * Use variable in the group and set the values in the 'Mask' of the group. This is the mask of the Wheel Motion group. You can edit mask in [Block\Edit Mask]. Left one is the name and right one is the variable. Double click the group and set the value.

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/0440226d-9d2a-40e0-913e-cbac708d795b)
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/7a59e187-34e7-4a82-bdbe-20791c015419)

---

## Whole boxes of simulink
### 1. Chassis block
### 2. Upper body block
### 3. L/B Bogies block
### 4. L/B Wheels block
### 5. Wheel velocity block
### 6. Stability Upper body block
### 7. Environments block
### 8. Robot position

---
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/91654e0d-3e12-4fde-bca9-b1873b389cc9)

* Set start point : Set the start point of the chassis
* Stability Upper body : Tilt sensor and contorller of upper body
* Upper body : Upper body and link of upper body and chassis
* Chassis : Chassis solid
* Environment : Test environments such as stairs, slopes and bumps
* L/R Bogie : Left and Right bogie and the wheels on the bogies
* L/R rear wheel : rear wheels
* Wheel Velocity : Controller of the wheels
  
### 1. Chassis block(Extrude Solid)
 * Geometry
   - Extrusion Type : General
   - Cross-section : [-0.3 0.2; -0.3 -0.068; -0.124 -0.068; -0.124 -0.23; 0.124 -0.23; 0.124 -0.068; 0.3 -0.068; 0.3 0.2]
     It have to be written the points with counter-clockwise. Each points are separated with ; like [x1,y1;x2,y2;x3,y3]
   - Length : 0.695
* Inertia
   - Type : Calculate from Geometry
   - Based on : Mass
   - Mass : 80
   -> Click Derived Values 'Update'
 * Graphic
    - color : whatever you want
 
   
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/abeaf998-ee97-4e1e-94ca-dff1b55b00ea)

* Frames (To connect with the other boxes)
  1) Click the surface, edge or point you want to choose
  2) Frame Origin/Based on Geometric Feature -> Click 'Use Selected Feature'
  3) Set the Frame Axes
  - Click F5 to update the feature and Click Apply!
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/ed046f76-01fb-4b48-8cb0-b5f8fbe72df4)

* Set start point block
  - This 6-DOF Joint makes the chassis stand right away.
    
### 2. Upper Body block

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/f577abf8-9887-4275-a549-663d23196f60)

   * Motor (Revolute Joint)
     - It makes F rotates based on the B and about the z-axis.
     - Set actuation torque as 'Automatically Computed'. It will be computed based on the motion input.
     - Set actuation motion as 'Provided by Input'. Input will be given in deg.
    
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/8f9e3824-eba6-4327-bb80-918652947c77)
    
  
  * Rigid Transformation
    - It moves the coordinate. Since the revolute joint is not on the top surface of the chassis, you have to move the coordinate of the joint.
    - The coordinate is moved base on the B so you have to choose which block you will put B.
    - Set the offset in Translation/Offset.
    - In this case, [-0.04 0 0] for both.

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/0f48243f-ddc4-4642-933d-cf436619b9a4)
    
  * Brick Solid
    - Link and Body are made with Brick Solid
    - It is similar with the Extrude Solid but simple.
    - Dimension of Link is [0.1 0.5 0.127] and Body is [0.7 0.6 0.75].

### 3. Bogies
* LB
  - This is the Rigid Transform to set the axis of the left bogie. B is the bottom center of the chassis and offset is [0.1 0.145 0.124]. In right case, the offset is [0.1 0.145 -0.124]. F is LB Joint.
* LB Joint
  - This is the Revolute Joint for connecting the left bogie to chassis. There is no torque because these are passive joint. And actuation motion is Automatically Computed.
  - Bogie have the resistance.
       
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/c82e4e14-b7f8-4c52-9b7d-f7ba639c9ab4)
  
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/78998a7a-3ab8-4034-8db1-683ee8dae111)
  
  - Put the Bound of the upper and lower limit as +14.5 and -14.5 deg.

* Bogie
  - Bogie is made with Exturuded Solid. Cross-Section is [-0.127 0.03; -0.17 -0.155; -0.13 -0.155; -0.088 -0.025; 0.088 -0.025; 0.13 -0.155; 0.17 -0.155; 0.127 0.03] and Length is 0.073m. [0,0] is the center of frame R so the axis of this is frame R
  - The offset of Rigid transform by F is [0 0 0.082] and the other is [0 0 0.06]
       
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/5451ba6a-c87d-4ae7-b4ff-ed288f417833)
      
### 4. Wheels
  
 * L-1
     - This is the Rigid Transform to set the axis of the left front wheel. B is the center of the left bogie and offset is [0.145 -0.1 0]. For left middle wheel [-0.145 -0.1 0]. Right wheels are also same for each position. But in rear wheels, B is the bottom center of the chassis so left offset is [0 -0.295 0.266] and right offset is [0 -0.295 -0.266].
 * Joint L-1
    - This revolute joint makes the rotation of wheels so these are provided actuation motion input and torques are automatically computed.

* Simscape Bus
   - It can help you connect many Spatial Contact Forces with one line. Each ports are connected with the port which have same name.


![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/069006d0-97f0-42e4-afcf-a574ffe4671c)
   
* Wheel L-1
  - This is the wheel made with the Cylindarical Solid.
  - The radius is 0.085 and length is 0.044.
  - To make it contact with the other blocks something like floor, stairs, slopes and bumps, you have to check Entire Geometry in Geometry/Export.
 
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/099cedee-cdcc-40e5-bfd1-56bccda369b5)

* Spatial Contact Force
     
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/85ca728d-3438-40fb-aed5-9df235668e68)

  - It makes two blocks can make normal force for each other.
   - You can set the Normal Force and Friction Force. For our poly urethane wheels, you can adjust 0.8 of static frictinon and 0.6 of dynamic friction.
 


 
### 5. Wheel Velocity
   - This is connected to the position input of the wheels. So the gradient of this is the angular velocity of the wheels. The angluar velocity of the wheel is (velocity of the chassis)/(radius of the wheel). So the function with the gradient which is same with velocity divided by 0.085 can be the position input of the wheels.
   - If you want to make it stop, the gradient should be 0. So you can add this function to previous function. Then the gradient will be 0 after the time of 'stop'.
     
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/8925d651-6db8-46ba-9436-f3aac4d75e06)
     
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/8216eb05-fe57-43af-b178-90d34cf7c7b5)
    
   - You can set the velocity in m/s and stop time in the Wheel Velocity mask. If you want to make it go backward, put minus value in velocity.
     
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/75ce3e97-093a-4610-a7c7-3bf96f1fc3a2)

     
### 6. Angle Sensor
  * 'Chassis angle' block is the tilt sensor of the chassis output (degrees) and the blocks on the right are used to calculate the inverse input of the motor in (degree).
    
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/f4d1160b-c161-43de-9873-c03d5865e985)

   * Transform Sensor
     - Senses the rotate angle of the chassis.
       
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/59c34956-918e-4bc7-ae73-b7e6fd34cc1e)
    
   * Rigid Transform
     - To make the input as -90~90 deg, change the axis of the base.
       
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/1d075631-2aa1-45fc-9fef-372a66b78c9f)

     
   * Substract and Multiply
     - To change the range of the output to -90~90, subtract 90.
     - Upper body should rotate the opposite way from the chassis, multiply -1.
   * Unit Delay
     - If you give the position input right away, it takes a lot of time to simulate. So give 0.005sec of delay to make output.
       
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/4094f482-7f32-4f70-8f50-b9046bb4ab57)


### 7. Environments

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/c2ea83ff-435f-4e45-b882-05f2ccbf86b3)

 * B is global and G makes contact with the wheel.
 * You can hide some blocks with Click and <Ctrl+Shift+X>. And to use certain environment, you have to match the number of the number of Spatial Contact Force blocks with this method.
 * Brick Solid
   - Solid for the floor. Dimension is [20 1 0.05]
 * Rigid Transform
   - Robot can be located under the floor so you have to move the floor to make the robot is on the floor.
#### 1) Slope test
  
 ![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/92ea8aec-1648-49d0-a05a-d08e894d7461)

  ![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/65a7f23d-bf7a-4943-bd60-e503630a0685)

* Floor is 'Entire Geometry' of the floor block.
* Extrude Solid
  - This is the solid for slope. Cross-section is [0 0; 2.174 0; 1.487 0.25; 0.687 0.25] for 20 deg.
  - Make new frame to put it right way
   
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/caa84652-4430-499c-b4c9-5efcdd3fc999)

* Position
  - Move the slope to make it located in front of the robot. Off set is [0.4 0 0]
* Turn
 
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/5c1a3f2e-bd49-4fd5-9523-787959d6236b)

  - Set the way with rotiation.
  * You can hide the blocks with <Ctrl+Shift+X> and choose the slope.
  * Hide G3-G10 of spatial contact force in each wheels.

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/b091c76d-2468-473f-9031-c9ec7a09a748)


#### 2) Staris test

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/ca85391b-451e-49ba-ba4d-929fd602397c)

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/f0b9a94c-9419-4fa9-af29-f17079302e12)

  * Position and Turn is same as the slope test
  * Stair
     - All of that is Rigid Transform and Birck Solid.
     - Each blocks are one step.
       
      | Off set | Parameter |
      | --- | --- |
      |[0.8 0 0.2] | [0.9 1 0.05] |
      |[0.6 0 0.15] | [1.3 1 0.05] |
      |[0.4 0 0.1] | [1.7 1 0.05] |
      |[02 0 0.05] | [2.1 1 0.05] |
      | - | [2.5 1 0.05] |
    
    - You have to set the frame like this.
   
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/f831c5c0-d65c-4e5d-9447-eebd27884537)

  * Hide G7-G10 of spatial contact force in each wheels.

![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/b9c5c038-8e99-47ff-98c9-04a175374c5c)

 #### 3) All Environment
  
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/e36b03c6-6d66-4743-b9c9-3126c8241255)
     
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/5e18f3e6-c0db-4ae2-9c45-e4956ccc61df)

  * You can make 4 ways with setting standard axis 180deg Turn1 and Turn2.
    - Turn1 is None -> Positive velocity
    - Turn1 is standard axis 180deg -> Negative velocity
  * Input of the solids Talbe
    
   |Name|Type of Solid|Offset|Dimension or (Cross-section/length)|Type of Frame|Else|
   |---|---|---|---|---|---|
   |Bump|Brick|[1 0 0.25]|[0.1 1.2 0.05]|A|15deg rotation|
   |Negative 1|Extruded|[0 0 0.2]|[-0.5 0; 0.5 0; 0.5 2.016; -0.5 2.284]/0.05|B||
   |Negative 2|Extruded|[3.2 0 0.2]|[-0.5 0; 0.5 0; 0.5 0.816; -0.5 1.084]|A||
   |Stair 5|Brick|[3.3 0 0.2]|[0.9 1 0.05]|A||
   |Stair 4|Brick|[0 0 0.15]|[4.4 1 0.05]|A||
   |Stair 3|Brick|[0 0 0.1]|[4.6 1 0.05]|A||
   |Stair 2|Brick|[0 0 0.05]|[4.8 1 0.05]|A||
   |Stair 1|Brick||[5 1 0.05]|A||
   |Slope|Extruded||[0 0; 0.25 0; 0.25 0.687]/1|B||
  - Type of Frame
    
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/2ba55e82-1b7c-48c9-8117-2d345060a084)

 * Full environment don't have to hide spatial contact force
   
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/f312f751-ab13-4699-8146-106dbe624673)

### 8. Robot position
* You can change the robot position based on the environments with changing the position of environments. In each environment blocks, there is turn block(in full environment, turn1 block). You can choose 'Rotation\Method' in that block. Choose 'None' if you want to make the robot moves forward and choose 'Standard Axis' and set 'Axis' as +Z, Angle as 180 if you want to make the robot moves backward.
  
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/4540653a-0308-4399-b6ff-08587f578ba8)
![image](https://github.com/ak5124/bogie_mobile_robot_simulation_tutorial/assets/78496021/79290873-c738-4c68-9a62-9f60914364e2)



     
