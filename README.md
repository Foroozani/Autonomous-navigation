# Autonomous navigation
## AI for Autonomous Vehicles - Build a Self-Driving Car

An example to build a 2D map inside a `Kivy` webapp. `Kivy` is a free and open source `Python` framework, used for the development of applications like games, or any kind of mobile app [website](https://kivy.org/#home). The whole environment for this project is built with `Kivy` and `PyTorch`, from start to finish.
 Here is the map of the car:

![image](https://github.com/Foroozani/Self_driving_car/blob/main/images/image1.png)

The sensors are there to detect the sand, so as to avoid it. The blue sensor covers an area at the left of the car, the red sensor covers an area at the front of the car, and the yellow sensor covers an area at the right of the car. Finally, there are three buttons to click on at the bottom left corner of the screen, which
are:
- _clear_: removes all the sand drawn on the map
- _save_: saves the weights (parameters) of the AI which is permanently trained
- _load_: loads the last saved weights that were saved before during the training


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

---
## Defining the goal 

The goal is to build a self-driving car which moves from airport to downtown where the upper left corner is the Airport, and the bottom right corner is Downtown. Now we can clearly formulate a goal, to train the self-driving car to make round trips  between the Airport and Downtown. As soon as it reaches the airport, it will then have to go to Downtown, and as soon as it reaches Downtown, it will then have to go to the Airport. More than that, it should be able to make these round trips along any road connecting these two locations. It should also be able to cope with any obstacles along that road it has to avoid


---
## Setting the parameters

Before you define the input states, the output actions and the rewards, you must set all the parameters of the map and the car that will be part of your environment. The inputs, outputs and rewards are all functions of these parameters. Let’s list them all, using the same names as in the code, so that you can easily understand the file “map.py”


1. **angle**: the angle between the x-axis of the map and the axis of the car
2. **rotation:** the last rotation made by the car (we will see later that when playing an action, the car makes a rotation)
3. **pos = (self.car.x, self.car.y):** the position of the car (self.car.x is the x-coordinate of the car, self.car.y is the y-coordinate of the car).
4. **velocity = (velocity_x, velocity_y):** the velocity vector of the car
5. **sensor1 = (sensor1_x, sensor1_y):** the position of the first sensor
6. **sensor2 = (sensor2_x, sensor2_y):** the position of the second sensor
7. **sensor3 = (sensor3_x, sensor3_y):** the position of the third sensor
8. **signal_1** the signal received by sensor 1
9. **signal_2** the signal received by sensor 2
10. **signal_3** the signal received by sensor 3


The last parameters, list below, are important because they’re the last pieces that we need to reveal the final input state vector
1. **goal_x:** the x-coordinate of the goal (which can either be the Airport or Downtown).

2. **goal_y:** the y-coordinate of the goal (which can either be the Airport or Downtown)

3. **xx = (goal_x - self.car.x):** the difference of x-coordinates between the goal and the car

4. **yy = (goal_y - self.car.y):** the difference of y-coordinates between the goal and the car.

5. **orientation:** the angle that measures the direction of the car with respect to the
goal

The goal has the coordinates (goal_x, goal_y) and the center of the car has the coordinates (self.car.x, self.car.y). For example, if the car is heading perfectly towards the goal, then orientation = 0°. If you’re curious as to how we can compute the angle between the two axes in Python, here’s the code that gets the orientation (lines 126, 127, 128 in the "map.py" file)

```python
xx = goal_x - self.car.x    # difference of x-coordinates between the goal and the car
yy = goal_y - self.car.y    # difference of y-coordinates between the goal and the car
orientation = Vector(*self.car.velocity).angle((xx,yy))/180.
```
--- 
## The inputparameters

The input state at each time is 
```python
last_signal = [self.car.signal1, self.car.signal2, self.car.signal3, orientation, -orientation]
```

--- 
## The output actions 
The Output Actions define them as the three following rotations: rotations = [turn 0° (i.e. move forward), turn 20° to the left, turn 20° to the right]

```python
# Getting our AI, which we call "brain", and that contains our neural network that represents our Q-function
brain = Dqn(5,3,0.9)           # 5 sensors, 3 actions, gama = 0.9
action2rotation = [0,20,-20]   # action = 0 => no rotation, action = 1 => rotate 20 degres, action = 2 => rotate -20 degres
```

In that case, one start a new if & else condition, saying that if the car has moved away from the destination, you give it a bad reward of -0.2, and, if the car has moved closer to the destination, you give it a good reward of 0.1. The way you measure if the car is getting away from or closer to the goal is by comparing two distances put into two separate variables: “last_distance” which is the previous distance between the car and the destination, at time t-1, and “distance”, which is the current distance between the car and the destination at time t. If you put all that together, you get the following code, which completes the above lines of code:

```python 
        if sand[int(self.car.x),int(self.car.y)] > 0: # if the car is on the sand
            self.car.velocity = Vector(1, 0).rotate(self.car.angle) # it is slowed down (speed = 1)
            last_reward = -1 # and reward = -1
        else: # otherwise
            self.car.velocity = Vector(6, 0).rotate(self.car.angle) # it goes to a normal speed (speed = 6)
            last_reward = -0.2 # and it gets bad reward (-0.2)
            if distance < last_distance: # however if it getting close to the goal
                last_reward = 0.1 # it still gets slightly positive reward 0.1
```
![](https://github.com/Foroozani/Self_driving_car/blob/main/images/image2.png)






















