# *6-Axis Manipulator*

*Timeline: February 2025 - Present*
## **Project Overview**

Welcome to my 6-Axis Manipulator project! This is an in-depth analysis of how to build a robotic manipulator from the ground up. I began this project from scratch with emphasis on every part of designing, constructing, and programming the manipulator.  

In this repository, you will see everything that I have completed to date, including significant notes, drawings, and technical information provided in the Notes & Basics folder. The intention is to leave a well-documented path for others interested in learning about the design of a 6-axis robotic manipulator.  
This project is extension of my previous project [4-Axis Manipulator](https://github.com/bhaarath22/4AxisManipulator.git)
## **Table of Contents**
- [Linear Diagram](#linear-diagram)
- [DH Table](#Denavit-HartenbergDH-parameters)
-  [Visualization of the Co-ordinate frames](#Visualization-of-the-Co-ordinate-Frames)
- [Transformation Matrices](#transformation-matrices)
- [Arm Matrix](#arm-matrix-t0e)
- [Position & Orientation Vectors](#Position-Vector)
- [Home Position](#Home-Position)

## *Linear Diagram*


![Free body Diagram](https://github.com/bhaarath22/6AxisManipulator/blob/1c44f4b05553427278d6e6f6ebbd0bd7ed504e72/Data/Frame%20arrangement%202.jpeg)
  ## *Projection of {6} on {0}*
![Position](https://github.com/bhaarath22/6AxisManipulator/blob/8ab5424e95ddc74ad68448610bd8e0c1f5eb9b1b/Data/Frame%20arrangement.jpeg)


# **Denavit-Hartenberg**(DH) parameters

| Link Length (a<sub>i-1</sub>) | Link Twist (α<sub>i-1</sub>) | Joint Offset (d<sub>i</sub>) | Joint Angle (θ<sub>i</sub>) |
|--------------------|-------------------|-------------------|-----------------|
| 0                |  π/2                 | d<sub>1</sub>     | θ<sub>1</sub> |
| a<sub>1</sub>    |0       | 0                 | θ<sub>2</sub> |
| a<sub>2</sub>    | 0                 | 0                 | θ<sub>3</sub> |
| a<sub>3</sub>    | π/2                | d<sub>2</sub>                 | θ<sub>4</sub> |
| 0    | -π/2            | d<sub>3</sub>     | θ<sub>5</sub> |
| 0                | 0            | d<sub>4</sub>     | θ<sub>6</sub> |

------
## **Visualization of the Co-ordinate Frames**
I used MATLAB (Robotics System Toolbox, Version 24.1, R2024a) to visualize the coordinate frames. This helped me view the joint axes in 3D and verify the correctness of my DH parameters. [code](Matlab/Visualisation-Co-ordinate-Frame.m)

![Data/Visualization.png](Data/Visualization.png)
----
## Transformation Matrices

### T<sub>01</sub>
```
[cos(th1), -sin(th1),  0,   0]
[       0,         0, -1, -d1]
[sin(th1),  cos(th1),  0,   0]
[       0,         0,  0,   1]
 
```

### T<sub>12</sub>
```
[cos(th2), -sin(th2), 0, a1]
[sin(th2),  cos(th2), 0,  0]
[       0,         0, 1,  0]
[       0,         0, 0,  1]

```

### T<sub>23</sub>
```
[cos(th3), -sin(th3), 0, a2]
[sin(th3),  cos(th3), 0,  0]
[       0,         0, 1,  0]
[       0,         0, 0,  1]

```

### T<sub>34</sub>
```
[cos(th4), -sin(th4),  0,  a3]
[       0,         0, -1, -d2]
[sin(th4),  cos(th4),  0,   0]
[       0,         0,  0,   1]

```

### T<sub>45</sub>
```
[ cos(th5), -sin(th5), 0,  0]
[        0,         0, 1, d3]
[-sin(th5), -cos(th5), 0,  0]
[        0,         0, 0,  1]


### T<sub>56</sub>
```
[ cos(th5), -sin(th5), 0,  0]
[        0,         0, 1, d3]
[-sin(th5), -cos(th5), 0,  0]
[        0,         0, 0,  1]

```



## Arm Matrix (T<sub>0E</sub>)
```
T0E =
 
[- cos(th6)*(sin(th1 + th2 + th3)*sin(th5) - cos(th1 + th2 + th3)*cos(th4)*cos(th5)) - sin(th6)*(sin(th1 + th2 + th3)*cos(th5) + cos(th1 + th2 + th3)*cos(th4)*sin(th5)), sin(th6)*(sin(th1 + th2 + th3)*sin(th5) - cos(th1 + th2 + th3)*cos(th4)*cos(th5)) - cos(th6)*(sin(th1 + th2 + th3)*cos(th5) + cos(th1 + th2 + th3)*cos(th4)*sin(th5)), -cos(th1 + th2 + th3)*sin(th4), a2*cos(th1 + th2) + a1*cos(th1) + a3*cos(th1 + th2 + th3) + d2*sin(th1 + th2 + th3) - d3*cos(th1 + th2 + th3)*sin(th4) - d4*cos(th1 + th2 + th3)*sin(th4)]
[                                                                                                                                               -cos(th5 + th6)*sin(th4),                                                                                                                                               sin(th5 + th6)*sin(th4),                      -cos(th4),                                                                                                                          - d1 - d3*cos(th4) - d4*cos(th4)]
[  cos(th6)*(cos(th1 + th2 + th3)*sin(th5) + sin(th1 + th2 + th3)*cos(th4)*cos(th5)) + sin(th6)*(cos(th1 + th2 + th3)*cos(th5) - sin(th1 + th2 + th3)*cos(th4)*sin(th5)), cos(th6)*(cos(th1 + th2 + th3)*cos(th5) - sin(th1 + th2 + th3)*cos(th4)*sin(th5)) - sin(th6)*(cos(th1 + th2 + th3)*sin(th5) + sin(th1 + th2 + th3)*cos(th4)*cos(th5)), -sin(th1 + th2 + th3)*sin(th4), a2*sin(th1 + th2) + a1*sin(th1) - d2*cos(th1 + th2 + th3) + a3*sin(th1 + th2 + th3) - d3*sin(th1 + th2 + th3)*sin(th4) - d4*sin(th1 + th2 + th3)*sin(th4)]
[                                                                                                                                                                      0,                                                                                                                                                                     0,                              0,                                                                                                                                                         1]
 
 
```
### Position Vector
P =
 ```
 
a2*cos(th1 + th2) + a1*cos(th1) + a3*cos(th1 + th2 + th3) + d2*sin(th1 + th2 + th3) - d3*cos(th1 + th2 + th3)*sin(th4) - d4*cos(th1 + th2 + th3)*sin(th4)
                                                                                                                         - d1 - d3*cos(th4) - d4*cos(th4)
a2*sin(th1 + th2) + a1*sin(th1) - d2*cos(th1 + th2 + th3) + a3*sin(th1 + th2 + th3) - d3*sin(th1 + th2 + th3)*sin(th4) - d4*sin(th1 + th2 + th3)*sin(th4)
 
 

```
## **Normal Vector (XN Axis)**
n =
``` 
- cos(th6)*(sin(th1 + th2 + th3)*sin(th5) - cos(th1 + th2 + th3)*cos(th4)*cos(th5)) - sin(th6)*(sin(th1 + th2 + th3)*cos(th5) + cos(th1 + th2 + th3)*cos(th4)*sin(th5))
                                                                                                                                               -cos(th5 + th6)*sin(th4)
  cos(th6)*(cos(th1 + th2 + th3)*sin(th5) + sin(th1 + th2 + th3)*cos(th4)*cos(th5)) + sin(th6)*(cos(th1 + th2 + th3)*cos(th5) - sin(th1 + th2 + th3)*cos(th4)*sin(th5))
 

 
 ```
## **Sliding Vector (YN Axis)**
s =
 ```
sin(th6)*(sin(th1 + th2 + th3)*sin(th5) - cos(th1 + th2 + th3)*cos(th4)*cos(th5)) - cos(th6)*(sin(th1 + th2 + th3)*cos(th5) + cos(th1 + th2 + th3)*cos(th4)*sin(th5))
                                                                                                                                              sin(th5 + th6)*sin(th4)
cos(th6)*(cos(th1 + th2 + th3)*cos(th5) - sin(th1 + th2 + th3)*cos(th4)*sin(th5)) - sin(th6)*(cos(th1 + th2 + th3)*sin(th5) + sin(th1 + th2 + th3)*cos(th4)*cos(th5))
 

```
## **Approach Vector (ZN Axis)**
a =
 ```
-cos(th1 + th2 + th3)*sin(th4)
                     -cos(th4)
-sin(th1 + th2 + th3)*sin(th4)



```
### ***Home Position***

The home position of the manipulator is when th1 =0; th2=0; th3=0; th4=0; th5=0; th6=0;

T0E =
```
[1, 0,  0,   a1 + a2 + a3]
[0, 0, -1, - d1 - d3 - d4]
[0, 1,  0,            -d2]
[0, 0,  0,              1]

```
I used the MATLAB ( Symbolic Math Toolbox  Version 24.1  (R2024a) ) to derive the forward kinematics solution and verified the manual calculations with MATLAB, confirming that they are correct.[code](Matlab/ForwardKinematics.m)

Projection of {6} on {0} is matching

P =
 ```
a1 + a2 + a3
- d1 - d3 - d4
           -d2

```
n =
 ```
 1
0
0
 ```
 
s =
``` 
0
0
1
 ```
 
a =
``` 
0
-1
0
```
 

