---
title: Automated Industrial Inspection employing Signal Processing and Computer Vision
image: /assets/images/projects/industry4_schema.png
# description: Mortgage Payment Calculation Panel App
# company: Personal Project
date: September 2020
layout: post
---

Automated industrial inspection (AII) is a critical component of Industry 4.0. Such automations enable to not only improve productivity, but also lead to significant improvements in operational safety. Mining is one of the few occupations with high employee risk. For example, in United States with its advanced operational safety had about [12 fatalities](https://arlweb.msha.gov/stats/centurystats/coalstats.asp) in 2019 from coal mining and [15 fatalities](https://arlweb.msha.gov/stats/centurystats/mnmstats.asp)  from metal/nonmetal mining. 

I was part of a small team of data scientists who worked with a leading potash mining corporation in North America to automate detection of geological features (clayseam) that can optimize potash extraction. The final solution will result in automous operation of mining machinery that will have a huge impact on the workplace occupational health and safety.

Our team adapted and optimized deep learning architectures (Unet family) to perform semantic segmentation. These architectures can help create models that can automatically detect clayseam from the input video signals. However, video signals from underground mining operations suffer from high optical 'noise' and poor lighting that degrades the
computer-vision's accuracy. As part of the project, we developed advanced  signal processing approaches employing global and local filters in both spatial (ex: histogram equalization) and spectral (ex: FFT transformations) domains to eliminate noise and
improve image quality. These improvements in image quality led to a marked improvement in the detection and accuracy with minimal addtion to computational expense.

The detection accuracy for the image-processing solution can be further improved if we can augment video signals with other
imaging signals such as infrared or LIDAR cameras. As part of this project, we developed a new deep learning-based solution that can simultaneously use data from multiple imaging modes that led to further improvements in detection and discrimation capabilities. The model traning and inference capabilities were built in cloud (AWS) but with containerization that enabled us to rapidly train the model in the cloud and use the same framework to deploy and test the model on edge devices. A template of the architecture used in shown below.


![AWS Architecture](/assets/images/projects/aws_architecture.png)
<center> <em> AWS Architecture for deep learning based solution </em> </center>
<br/>

A sample output of the semantic segmentation output from detecting clayseam is shown below
<br/><br/>

![Clayseam running through potash mining tunnel](/assets/images/projects/clayseam.png)
<center> 
<em> Clayseam automatically detected in mining tunnel </em> </center>

Currently, I'm working along with my team to repurpose this solution along with other components to automatically inspect defects such as cracks, fractures, blockages in wastewater distribution networks for a major customer in North America as shown in a sample output below.

![Sewer pipe inspection ](/assets/images/projects/sewer_pipe_sample.png)
<center> 
<em> Automated inspection of  Waste water distribution pipes </em> </center>


