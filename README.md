# Self_driving_car
## AI for Autonomous Vehicles - Build a Self-Driving Car

An example to build a 2D map inside a `Kivy` webapp. `Kivy` is a free and open source `Python` framework, used for the development of applications like games, or any kind of mobile app [website](https://kivy.org/#home). The whole environment for this project is built with `Kivy` and `PyTorch`, from start to finish.
 Here is the map of the car:

![image](https://github.com/Foroozani/Self_driving_car/blob/main/images/image1.png)


### Building Environment 
---
* [x] Installation 

Use the terminal or an Anaconda Prompt for the following steps:

1. To create an environment:
```bash 
conda create --name ai36
```
2. When conda asks you to proceed, type *y*:
This creates the **ai36** environment in /envs/. No packages will be installed in this environment.

3. To create an environment with a specific version of Python:
```bash 
conda create -n myenv python=3.6
```
Once it's created, go to the Anaconda Navigator window, and look for a box that says Applications on "name", select the new one. (This will erase all libaries so we must re install them again)

- Install Spyder, by simply clicking "Install" in the same window and opening it.

4. Now on the command prompt of anaconda run the following commands (On Mac or Linux)

```bash 
conda install numpy
conda install matplotlib
conda install pytorch torchvision -c pytorch
pip install kivy
```
- Then we can fix the versions, so do the following commands

```bash 
pip install torchvision==0.1.6
pip install torch==0.3.1
```
5. Run the "map_commented.py".


* [x] Defining the goal



























