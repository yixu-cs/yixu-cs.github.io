---
title: "RoboGame2020 Competition"
role: "Participant."
start: 2020-06-01
end: 2020-10-01
excerpt: "Collaborated in designing and programming a robot with STM32 embedded systems for moving simulated patients to designated beds; programmed and controlled the robotic arm for task execution. Achieved <b>Fourth Place</b> in the second round of the competition.<br/><img src='/images/projects/robo-hardware.png' width='400' />"
collection: projects
---

**Gains:** in this competition, I experienced building a robot’s hardware from scratch and doing embedded programming. Embedded programming is about using software to control hardware, and I learned many unfamiliar concepts such as timing-based programming and voltage levels. Since I was only a freshman at the time, my involvement in this project was limited; I mainly contributed by helping other three students build the robot, and debugging code to control the movement of the robot’s arm.

Our task
------

<p align="center">
  <img src="/images/projects/robo-background.JPG" width="400"/>
</p>

In the competition, there were blocks of different colors placed on shelves to simulate patients. We needed to control the robot to move three yellow "patient" to three empty shelf, and transport the red "patient" to the designated shelf in the intensive care unit, then put a small mask on the red patient. In addition to color differences, each patient also had an RFID tag. We also had to accomplish all these tasks under time constraints.

Therefore, our robot needed to be capable of line-following (which could be implemented using a camera or a grayscale sensor), identifying patients (by recognizing colors with a camera or by reading RFID tags), lifting and placing patients, and grasping and placing masks.

Our design
------

The robot has three main parts: the wheel-driven mobility section, a baseplate with modules, and the robotic arm. 
* The baseplate was designed to be as large as possible within the allowed limits, which helped minimize the robot’s movements.  
* The robotic arm looked like a forklift, ensuring that the number of motors and joints is minimized, so as to increase stability. At the front of the arm, we designed a mechanical gripper for grasping masks.  
* On the base, there were three omni-directional wheels positioned at 120-degree angles, each connected to a DC gear motor. This is a classic and efficient design for movement, enabling the robot to move forward and backward through simple force synthesis.

Hardware design         |  Arm under the patient | Arm lifted
:-------------------------:|:-------------------------:|:-------------------------:
<img src='/images/projects/robo-hardware.png' width='300' />  |  <img src='/images/projects/robo-arm1.png' width='300' /> | <img src='/images/projects/robo-arm2.png' width='300' />


For line-following, since the venue had black solid lines on a white background, we used four grayscale sensors to detect whether the robot had deviated from the black track or not. 
If it did, we adjusted the wheels to steer the robot back using velocity synthesis. 

We identified each patient's RFID one by one. When the robot detected a patient's RFID, it stopped. Since our robotic arm was slightly lower than the "patient", if the detected patient was our target, we used servos to control the arm, rotating and lifting it to move the patient.

<p align="center">
  <img src="/images/projects/robo-navigate.png" width="500"/>
</p>