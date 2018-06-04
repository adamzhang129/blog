---
layout: post
title: Use Force torque sensor in ubuntu.
date: 2018-06-04 12:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: we-in-rest.jpg # Add image post (optional)
tags: [Force torque sensor, linux] # add tag
---

### Motivation

For the need in research, I have to work with force torque sensor **Mini 27** from ATI industry in ubuntu system.
This post serve as a reminder and note of my exploration and progression to fulfill the need in this scenario.


### System and environment

> * ubuntu 16.04


### Get MUJOCO set up
Download mjpro150 on https://www.roboti.us/index.html and also get your licence on https://www.roboti.us/license.html .
Be aware of that one license can be used for multiple version mujoco. MUJOCO provide student license for 1 year trial, use institute 
mail to register.

After downloading, uzip it to **~/.mujoco/mjp150** and move the downloaded key file (usually named **mjkey.txt**) to 
**~/.mujoco/mjp150/bin** & **~/.mujoco** and then run sample code in **bin**directory.

Then run in terminal:

```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/adam/.mujoco/mjpro150/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia-390
```


or put then in ~/.bashrc

### Install mujoco_py

Download from https://github.com/openai/mujoco-py and the install using

```bash
sudo pip3 install --no-cache-dir -e .

echo "export LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libGLEW.so:/usr/lib/nvidia-390/libGL.so" >> ~/.bashrc
```


this installation code can solve **GLEW initalization error: Missing GL version** error when importing.
another workaround I saw is to reduce you version of mujoco_py 1.3 and cooreponding gym MUJOCO version need to be 
adjusted.


BTW you need to 

```
sudo apt-get install libglew-dev libglfw-dev
```

before importing, otherwise you won't pass compiling.

by now you can run 

```
env.render()
```

in your code or you can test with mujoco_py/examples.

### Reference

[1](https://link.zhihu.com/?target=https%3A//github.com/openai/mujoco-py/pull/145)

[2](https://github.com/openai/mujoco-py/issues/44)
