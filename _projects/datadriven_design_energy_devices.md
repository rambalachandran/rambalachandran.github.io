---
title: Data driven design of Clean Energy Devices
image: /assets/images/projects/energy_devices.png
# description: Mortgage Payment Calculation Panel App
# company: Personal Project
date: April 2019
layout: post
---

## Modeling Approaches to design Energy Devices
There is a critical need to rapidly increase the adaptation of clean energy technologies to mitigate climate change. This in turn requires continuous optimization of clean energy devices that has better performance while being manufactured and operated at lower cost. Physics based mathematical models are typically used to simulate the operation of these devices that in turn helps in designing better performing devices. In my professional career, I have worked on developing such models for a wide range of clean energy devices ranging from thermoelectrics, batteries and fuel cells.

![BoPGD COM](/assets/images/projects/energy_devices.png)
<center> <em> Clean Energy Devices that I have worked on </em> </center>
<br/>

## Challenges of using Machine Learning to design Energy Devices
However, simulating these models is computationally very expensive. This limits their applicability to a small search space. The recent advancements in data science and machine learning provide an alternative approach to build computationally inexpensive models that can learn from existing data. However, the major challenges in using machine learning for designing and optimizing energy devices are
1. Limited availability of dataset, and 
2. Explainable models that provide explicit functional relationships between input features and the target outcome. 

## Integrating Machine Learning with subject matter knowledge
To overcome this challenge, me and my team members came up with a novel approach in which the machine learning algorithm (Bootstrapped
projected gradient descent -- BoPGD) is constrained with subject matter knowledge such as dimensional analysis and scaling law relationships between input descriptors. This constrained learning model enables us to learn from small data and develop predictive models that are accurate, computationally inexpensive, and physically interpretable. We demonstrated how this approach can be used to design better supercapacitor components. The results are published in a peer reviewed [journal article](https://doi.org/10.1021/acs.chemmater.8b02837). This article was [recognized](https://doi.org/10.1021/acs.chemmater.9b03854) by the journal as one of the most important machine learning publications of the year.

![BoPGD COM](/assets/images/projects/bopgd_com.png)
<center> <em> Schema of the hybrid approach that constraints machine learning model with subject matter knowledge </em> </center>
<br/>

## Generalized Graph based Deep Learning Models
On the other end, me and my team members and collaborators worked on developing generalized graph based deep learning models with multi-tasking that can work simultaenously to design components of different energy devices that require optimization of different functionalities. This model achieved unprecedented accuracy over a wide range of potential clean energy components compared to other existing models. As a result, this work was [published](https://arxiv.org/abs/1811.05660) in a [workshop](http://www.quantum-machine.org/workshops/nips2018/) in the Neural Information Processing Systems 2018, one of the top machine-learning conferences in the world.

![CGCNN MT](/assets/images/projects/cgcnn_mt.png)
<center> <em> Graph based deep learning models with multi tasking</em> </center>
<br/>

## Conclusion
Adapting machine learning and data science approaches to new areas to create effective insights requires a deep understanding of these approaches and the subject matter knowledge. In this project, me and my team members demonstrated how such adaptation can be made to designing components of clean energy devices to accelerate development and deployment of next generation energy devices.




