---
title: "KUKA youBot Mobile Manipulation"
author_profile: true
key: 5
excerpt: "Trajectory planning, Feedforward control"
header:
  teaser: /assets/images/KUKA_best_run.gif
classes: wide
---

{% comment %} 
sidebar:
  - title: "Role"
    image: http://placehold.it/350x250
    image_alt: "logo"
    text: "Designer, Front-End Developer"
  - title: "Responsibilities"
    text: "Reuters try PR stupid commenters should isn't a business model"
{% endcomment %} 

The goal of this project is to showcase the fundamentals of robotic manipulation having the KUKA youBot move to pick up a cube from a known location and place it at another. The project plans a trajectory for the end-effector, arm, and chassis of the robot and uses a PI control loop with odometry to have the robot correctly complete the task. 

## Video Demo
<iframe
    width="100%"
    height="50px"
    src="https://www.youtube.com/embed/UkNCx6J6GJc"
    frameborder="0"
    allow="autoplay; encrypted-media"
    allowfullscreen
>
</iframe>

## Reference Trajectory Generation
To create the motion of the robot, I created the end-effector trajectory consisting of 8 distinct trajectory segments:
  1. Move the gripper from its initial configuration to a "standoff" position above the cube.
  2. Move the gripper down to a grasping position from the cube.
  3. Close the gripper to grasp the cube.
  4. Return to the first "standoff" position now holding the cube.
  5. Move to the "standoff" position above the final position of the cube.
  6. Move the gripper down to place the cube on the ground.
  7. Release the cube from the gripper.
  8. Return to the final "standoff" position above the cube.

As a part of this challenge, I created two unique trajectories with different initial and final placements of the cube to show robustness of the control of the robot.

## youBot Kinematics Simulation
To calculate the kinematics and simulate the manipulation of the robot, I wrote two main functions. The first function uses simple Euler integration to calculate the next state of the robot chassis, arm, and wheel joints using the following inputs:
- The current configuration of the robot
- The current joint velocities
- The maximum velocity for the joints
- The timestep for the calculations

 This function then returns the configuration of the robot for the next timestep.

The second function I wrote serves as the control loop that ties the desired trajectory of the end effector to the actual motion of the end effector. A simple diagram of the control loop is seen below:
![control_loop]({{ site.url }}{{ site.baseurl }}/assets/images/youBot_control.png)

## Feedforward Control
To calculate the control law, we need the current actual end-effector configuration X(q,θ), a function of the chassis configuration q and the arm configuration θ. The values (q,θ) come directly from the simulation results in the last section. In other words, assume perfect sensors.

The output of this part is the commanded end-effector twist expressed in the end-effector frame. To turn this into commanded wheel and arm joint speeds, we use the pseudoinverse of the mobile manipulator Jacobian Je(θ). 

<!-- ## Source code
[Github repo](https://github.com/hang-yin/Mobile_Manipulation) -->