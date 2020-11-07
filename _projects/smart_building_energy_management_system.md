---
title: Low cost prototype of Smart Building Energy Management System (s-BEMS).
image: /assets/images/projects/australia_ghg.png
# description: Mortgage Payment Calculation Panel App
# company: Personal Project
date: April 2020
layout: post
---
## Why need a low cost s-BEMS?
Globally, buildings are a major contributor  to greenhouse gas (GHG) emissions. For example, in Australia, buildings consume 20% of the 
energy and contribute to ~ 23% of GHG emissions. Improving energy efficiency of buildings has a critical role towards achieving the Australian government’s target of reducing GHG emissions by 26-28% in 2030.

![Australia GHG](/assets/images/projects/australia_ghg.png)
<!-- <center> <em> The various critical components of Industry 4.0 </em> </center>
<br/> --> 


## Why s-BEMS?
A low cost predictive solution that combines inexpensive edge devices to state-of-art AI/ML approaches can automatically detect major thermal loads in buildings. These solutions can be retrofitted and tailored to the needs of individual buildings in particular older buildings. This information can be used to adaptively control building energy utilization.

## What is s-BEMS?
As part of an internal project, I lead a small team of data scientists to build a low cost s-BEMS prototype. This prototype was able to collect sensor data from low cost edge devices such as Raspberry Pi. The external weather and environmental data are obtained from a weather API. The video streaming data from the edge device was processed using computer vision approaches to automatically detect the various sources of thermal loads such as number of people, lights,  computers etc. All the collected, processed and predicted data are stored in a centralized opensource IoT platform (Thingsboard).

![R-C Model](/assets/images/projects/trc_model.png)
<center> <em> The various critical components of s-BEMS </em> </center>
<br/>

The predictions along with internal and external conditions data is fed to a physics based Thermal Resistor Capacitance (R-C Model) to predict the new setpoint temperature that can be used to adaptively control the building heating ventilation and air conditioning (HVAC) systems. 

## What are the next steps?
We are working towards further optimizing the solution to include (i) forecasting models to forecast internal sensor data, and (ii) Develop AI/ML models that are learn from the physics based models to predict the internal setpoint temperatures. Furthermore, the solution needs will be customized and tailored to different customer expectations of deploying them in local host machines or remote cloud or a hybrid deployment.






