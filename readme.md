![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/9eec25c2-7917-4fd9-9a37-cae6d15ef1ec)

# The parameters of the Vestek robot 
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/0a564183-6570-47a4-b86f-174fa8a83f62)

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

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/dcbd89ff-da3a-4e95-91f5-43672c518c97)![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/9787e05e-0534-4afe-85c7-abf4e1ba48e3)


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
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/007a65bf-0072-4940-8f1d-3c583ebbedf4)

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
 
   
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/b527f413-8e5c-4b80-8de2-571fbe8ca81a)

* Frames (To connect with the other boxes)
  1) Click the surface, edge or point you want to choose
  2) Frame Origin/Based on Geometric Feature -> Click 'Use Selected Feature'
  3) Set the Frame Axes
  - Click F5 to update the feature and Click Apply!
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/a7ed6b3e-9721-4d77-ad68-08d5ddfdc321)
 
* Set start point block
  - This 6-DOF Joint makes the chassis stand right away.
    
### 2. Upper Body block

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/8c8d0e63-021a-4cee-859c-1b3e13927844)

   * Motor (Revolute Joint)
     - It makes F rotates based on the B and about the z-axis.
     - Set actuation torque as 'Automatically Computed'. It will be computed based on the motion input.
     - Set actuation motion as 'Provided by Input'. Input will be given in deg.
    
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/31ddb6c2-0750-4af9-8d2e-da2f936a6b52)
     
  
  * Rigid Transformation
    - It moves the coordinate. Since the revolute joint is not on the top surface of the chassis, you have to move the coordinate of the joint.
    - The coordinate is moved base on the B so you have to choose which block you will put B.
    - Set the offset in Translation/Offset.
    - In this case, [-0.04 0 0] for both.

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/259c95ce-fa05-4958-86dd-ce4b73f8c5c6)
    
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
       
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/c6b4eb9e-a75f-4d5b-9cdc-aa5fb304104c)!
   
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/d6ce6098-30d5-4d5d-87f9-45f4901935e7)
  
  - Put the Bound of the upper and lower limit as +14.5 and -14.5 deg.

* Bogie
  - Bogie is made with Exturuded Solid. Cross-Section is [-0.127 0.03; -0.17 -0.155; -0.13 -0.155; -0.088 -0.025; 0.088 -0.025; 0.13 -0.155; 0.17 -0.155; 0.127 0.03] and Length is 0.073m. [0,0] is the center of frame R so the axis of this is frame R
  - The offset of Rigid transform by F is [0 0 0.082] and the other is [0 0 0.06]
       
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/e1220919-2470-40b3-84de-9b99579e20c6)
       
### 4. Wheels
  
 * L-1
     - This is the Rigid Transform to set the axis of the left front wheel. B is the center of the left bogie and offset is [0.145 -0.1 0]. For left middle wheel [-0.145 -0.1 0]. Right wheels are also same for each position. But in rear wheels, B is the bottom center of the chassis so left offset is [0 -0.295 0.266] and right offset is [0 -0.295 -0.266].
 * Joint L-1
    - This revolute joint makes the rotation of wheels so these are provided actuation motion input and torques are automatically computed.

* Simscape Bus
   - It can help you connect many Spatial Contact Forces with one line. Each ports are connected with the port which have same name.


     ![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/a1f32866-9198-44c5-abd1-1a27846126a5)
     
* Wheel L-1
  - This is the wheel made with the Cylindarical Solid.
  - The radius is 0.085 and length is 0.044.
  - To make it contact with the other blocks something like floor, stairs, slopes and bumps, you have to check Entire Geometry in Geometry/Export.
 
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/361cd52a-9982-42aa-9189-d4d57af3fa68)

* Spatial Contact Force
     
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/d58c02bb-24ea-4b9a-b4fe-1d9c8e6222bf)

  - It makes two blocks can make normal force for each other.
   - You can set the Normal Force and Friction Force. For our poly urethane wheels, you can adjust 0.8 of static frictinon and 0.6 of dynamic friction.
 


 
### 5. Wheel Velocity
   - This is connected to the position input of the wheels. So the gradient of this is the angular velocity of the wheels. The angluar velocity of the wheel is (velocity of the chassis)/(radius of the wheel). So the function with the gradient which is same with velocity divided by 0.085 can be the position input of the wheels.
   - If you want to make it stop, the gradient should be 0. So you can add this function to previous function. Then the gradient will be 0 after the time of 'stop'.
     
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/1884d8a0-f130-45d9-b0be-38806bde024b)
     
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/1edba37c-2aeb-4caa-9151-255fb2cf90e7)
     
   - You can set the velocity in m/s and stop time in the Wheel Velocity mask. If you want to make it go backward, put minus value in velocity.
     
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/50a182a3-f1fe-452b-b488-124ccb58d1b4)

     
### 6. Angle Sensor
  * 'Chassis angle' block is the tilt sensor of the chassis output (degrees) and the blocks on the right are used to calculate the inverse input of the motor in (degree).
    
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/f9d1ce17-9c35-48fd-967e-8eb1f01c54d3)

   * Transform Sensor
     - Senses the rotate angle of the chassis.
       
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/80422547-5845-45b5-b748-597f75388e19)
     
   * Rigid Transform
     - To make the input as -90~90 deg, change the axis of the base.
       
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/5022b8c8-eb46-4167-9831-09726529e507)

     
   * Substract and Multiply
     - To change the range of the output to -90~90, subtract 90.
     - Upper body should rotate the opposite way from the chassis, multiply -1.
   * Unit Delay
     - If you give the position input right away, it takes a lot of time to simulate. So give 0.005sec of delay to make output.
       
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/7cc7a503-c699-4f22-9ef0-2c12d7640eec)


### 7. Environments

 ![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/baa2aa8e-0664-4b40-b8ff-7fd3a821da02)
 * B is global and G makes contact with the wheel.
 * You can hide some blocks with Click and <Ctrl+Shift+X>. And to use certain environment, you have to match the number of the number of Spatial Contact Force blocks with this method.
 * Brick Solid
   - Solid for the floor. Dimension is [20 1 0.05]
 * Rigid Transform
   - Robot can be located under the floor so you have to move the floor to make the robot is on the floor.
#### 1) Slope test
  
  ![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/46aa1532-651c-46b4-ab2e-72d506cdfd96)

  ![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/953479c6-4941-4ae0-a080-338a65d918c3)

* Floor is 'Entire Geometry' of the floor block.
* Extrude Solid
  - This is the solid for slope. Cross-section is [0 0; 2.174 0; 1.487 0.25; 0.687 0.25] for 20 deg.
  - Make new frame to put it right way
   
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/62a55670-2e9b-42bd-b60b-f1cf105278dd)

* Position
  - Move the slope to make it located in front of the robot. Off set is [0.4 0 0]
* Turn
 
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/b6766271-36ca-480a-bacb-7f95c110fe86)

  - Set the way with rotiation.
  * You can hide the blocks with <Ctrl+Shift+X> and choose the slope.
  * Hide G3-G10 of spatial contact force in each wheels.

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/68f0e217-1613-4c1e-891e-e5a14f2f30bd)


#### 2) Staris test

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/cfa260d8-499c-4db3-954d-3343fedb31b1)

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/06574ba4-ae1e-4f02-a067-a7f31b3a2cde)

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
   
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/887d7dd8-8073-4cd6-b4e3-b2fe0205e7ef)

  * Hide G7-G10 of spatial contact force in each wheels.

![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/18f70537-cf5a-4b1e-b061-07d4eae9d579)

 #### 3) All Environment
  
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/519a1bdd-f193-4feb-9764-73feb7ff7403)
      
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/2821aab9-eb7c-4a1f-953e-312030d1317e)

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
    
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/c3645bc6-b183-484d-9e19-eed829481049)

 * Full environment don't have to hide spatial contact force
   
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/5b7ca7f5-b64b-4598-9dbb-04d58317da76)

### 8. Robot position
* You can change the robot position based on the environments with changing the position of environments. In each environment blocks, there is turn block(in full environment, turn1 block). You can choose 'Rotation\Method' in that block. Choose 'None' if you want to make the robot moves forward and choose 'Standard Axis' and set 'Axis' as +Z, Angle as 180 if you want to make the robot moves backward.
  
![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/b143706e-5fd7-44fe-9be5-4ced190c9924)  ![image](https://github.com/nabihandres/vestek_matlab_simscape/assets/78496021/3222618c-8aba-4fc9-b2e4-22693d06c8b9)


     
