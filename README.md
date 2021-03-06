# Game Engines 2 2017-2018

[![Video](http://img.youtube.com/vi/n3uJ--m3f6Y/0.jpg)](http://www.youtube.com/watch?v=n3uJ--m3f6Y)

## Resources
- [Class Facebook page](https://www.facebook.com/groups/153206655393239/)
- [Course Notes](https://drive.google.com/drive/folders/1HbqIE6_hwy0osMKDmEG5GgOAUeGy7NXV?usp=sharing)
- [Previous lab tests](https://1drv.ms/u/s!Ak7y2552PWCrkNACJ7n8qiU8UPRs9w)
- [Assignment](ca.md)
- [A build of Infinite Forms](https://drive.google.com/file/d/1w24BcMAi6P1XmPc9D9ss6Lkro4KBvsMS/view?usp=sharing)
- [Forms git repo - Please Fork!](https://github.com/skooter500/Forms)
- [A spotify playlist of music to listen to while coding](https://open.spotify.com/user/1155805407/playlist/5NYFsIFTgNOI93hONLbqNI)
- [Exploring the Psychedelic Experience through Virtual Reality](https://www.facebook.com/askdirectdublin/videos/10155614575684395/)

## Contact the lecturer

- Web: http://bryanduggan.org
- Email: bryan.duggan@dit.ie
- Twitter: [@skooter500](http://twitter.com/skooter500)

## Web Resources
- [Unity Tutorials](https://unity3d.com/learn/tutorials) 
- [AI Game Dev](http://aigamedev.com/)
- [GDC Vault](http://www.gdcvault.com/)
- [Game maths tutorials](http://www.wildbunny.co.uk/blog/vector-maths-a-primer-for-games-programmers/)
- [Behavior trees in C#](https://github.com/BraveSirAndrew/DisciplineOak)

## Previous years courses
- 2016-2017
	- http://github.com/skooter500/GAI-2017
	- https://github.com/skooter500/GE2-Supplemental-2017
	- https://github.com/skooter500/GE2-LabTest2-2017
	- https://github.com/skooter500/OrbitalRingsSolution
- 2015-2016
    - https://github.com/skooter500/gameengines2015
    - https://github.com/skooter500/BGE4Unity
- 2014-2015
    - https://github.com/skooter500/BGE
    - https://github.com/skooter500/UnitySteeringBehaviours 
- 2013-2014
    - https://github.com/skooter500/XNA-3D-Steering-Behaviours-for-Space-Ships
	
## Assessment Schedule	
- Week 4 - CA proposal & Git repo - 10%
- Week 7 - Lab test - 20%
- Week 12 - CA Submission & Demo - 50%
- Exam - 30%

# Week 8 - Obstacle Avoidance
- How are the feelers calculated?
- What direction do they point?
- How many are there? Whys this number?
- How does the behaviour handle holes in the colliders?
- Does the speed of the boid matter?
- What direction is the force generated?
- WHat alternatives are there to this?
- How does the behaviour avoid certain obstacles while ignoring others?
- How often are the feelers calculated and why?
- How does the behaviour take priority over other behaviours?
- How would you improve the behaviour?
- How does it differ from Craig Reynolds avoidance behaviours?

![Image](images/IMAG0190.jpg)

# Lab
Check out scene6 for an example of obstacle avoidance with seek. To fully explore the awesome power of this behaviour, set up a scene with a spawner object that creates lots of creatures with wander, harmonic and obstacle avoidance and the spine animation procedural animation from a prefab inside a sphere with some obstacles and watch the creatures explore the environment without crashing into each other or the environment. You can set up seperate layers for the environment and for the creatures and set the layermask on the avoidance behaviour. Don't forget to set the layermask on each of the creatures body parts. Also you will have to adjust the max force, max speed, weights and priorities to get the perfect behaviour.

# Week 7 - Harmoinc steering behaviours
- [Harmonic motion](http://hyperphysics.phy-astr.gsu.edu/hbase/shm.html)
- [Procedural animation](https://www.roadtovr.com/the-need-for-procedural-animation-in-virtual-reality-ocean-rift-developer/3/)

## Lab

Check out scene5 for an example of the jitter wander behaviour we developed last week. In today's lab we will create a steering behaviour that can be used to make all kinds of swimming creatures. Open scene6 and there are 3 "creatures". The creature on the left uses the wander steering behaviour. The other two creatures have a behaviour called Harmonic attached that currently does nothing. This is what the creatures should look like after you implement the Harmoinc behaviour:

[![YouTube](http://img.youtube.com/vi/noL6Z3US2Gc/0.jpg)](https://www.youtube.com/watch?v=noL6Z3US2Gc)

The middle creature is seeking a point on a sphere projected in front of the boid that oscillates on the horizontal plane. The right creature is seeking a point on a sphere that oscillates on the vertical plane. 

You might find this processing sketch useful in working out how to implement these behaviours: 

```Java
void setup()
{
  size(500, 500);
  cx = width / 2;
  cy = height / 2;
}

float freq = 1.0f;
float theta = 0;
float amplitude = 60;
float radius = 200;
float cx, cy;
float timeDelta = 1 / 60.0f;
void draw()
{
  background(0);
  float angle = sin(theta) * radians(amplitude);
  float x = cx + sin(angle) * radius;
  float y = cy + cos(angle) * radius;
  noStroke();
  fill(255);
  ellipse(x, y, 50, 50);
  theta += TWO_PI * freq * timeDelta;
}
```

Advanced! 

You can add more variety to the Harmonic behaviour by using either of these two techniques:

- Use Perlin noise to generate the oscillation rather than harmonic motion. 
- Write an additional component called a HarmonicController that changes the frequency and amplitude every few seconds in a co-routine. You should lerp to the new values.

# Week 6
- A better integration function
- A better steering behaviours framework that uses components
- [Submit the first CA deliverable](https://docs.google.com/forms/u/1/d/e/1FAIpQLScz8S3HAHrQkhlDSV41hifpS2lDyQt6na3TTlZ2a_hkz_ZNDw/viewform)

## Lab
### Learning Outcomes
- Refactor the code to use components
- Use the pursue steering behaviour to set up a predator/prey simulation


## Task 1
Out Boid class is getting big so now it's time to refactor the code so that each behaviour is a seperate component.

- Make an abstract base class called SteeringBehaviour that extends MonoBehavior
- Add one abstract method ```public abstract Vector3 Calculate()```
- Add a public field for *weight* of type float and also add a field for the Boid
- Take each of the behaviours we wrote (seek, arrive, flee, pursue, offsetpursue) and make each of them extend SteeringBehaviour. Do the calculation of the force in Calculate
- Give the Boid a field of type ```SteeringBehaviour[]```
- In the setup method of the Boid class, call GetComponents to get all the attached SteeringBehaviours
- Assign the boid field of each attached behaviour
- In the Boid Update method, iterate over the activated behaviours and sum the generated forces * the weights
- This will become more useful when we decide to combine the behaviours together

## Task 2

Try and make this predator prey simulation. The prey will follow a path until the predator comes into range. When the perdator is is range the prey will attack the predator by shooting at it. It only shoots at the predator if it is inside the field of view. The predator will get close to the prey, but will flee from the prey if the prey attacks it:

[![YouTube](http://img.youtube.com/vi/SqThPN_ogJE/0.jpg)](https://www.youtube.com/watch?v=SqThPN_ogJE)


# Week 5 - Pursue & offset pursue, also behaviours with components
- Pursue & offset pursue

## Lab
## Learning outcomes
- Discover the limits of Unity physics
- Use the equations of motion to simulate some stuff
- Maybe use the Unity profiler!

### Task 1

Check out the seek, arrive and flee behaviours we made in the class last week. You can open scene4.unity to find a scene that uses them and you will find the code in Boid.cs

Modify the scene to use a Rigidbody to perform the integration of the calculated force and see if you can achieve the same effect as our custom integration function. Later we will add banking. This might be tricky to do with a rigidbody (or it might not I don't really know!).

### Task 2

The height of the empire state building is 381m. Acceleration due to the force of gravity is 9.8m/s/s. Write c# code to calculate how long it will take for an object to fall to ground level from the top of the empire state building. You can use this equation:

∆s = v<sub>0</sub>t + ½at<sup>2</sup>

Where ∆s is the distance travelled, v<sub>0</sub> is the starting speed, t is time and a is acceleration. Here are [some notes that explain how this equation is derived](https://1drv.ms/p/s!Ak7y2552PWCrjNho5TTf_aHBLOMy0Q).

Now, create a scene and simulate the object falling using:

- A Rigidbody
- An Euler integration function

And see if the time taken in the simulations matches the ideal time calculated using the equation.

### Task 3

Send me your github username and I will add you to the Forms repo. You can clone the repo. To get the project to compile and run, you have to add the SteamVR thing from the Unity assets store. Use the Unity profiler to profile the running game and see if you can come up with any ideas about how I can improve the frame rate. You can try replacing the integration function in Boid.cs with a Rigidbody for example. Special gold star to anyone who comes up with some ways to optimise the code today! I will be doing visuals at the [Energy Collective party this Saturday](https://www.facebook.com/events/185048675577427/) in the Voodoo Lounge with the project from 11pm-3am. Welcome to come along!

# Week 4 - Introduction to steering behaviours
- Lecture notes
- [Craig Reynolds original paper](https://www.red3d.com/cwr/papers/1999/gdc99steer.pdf)
- [Steering behaviours in Java](https://www.red3d.com/cwr/steer/)
- We implemented seek arrive and flee steering behaviours in the class. Check out scene4.unity

## Lab
## Learning Outcomes
- Learn how to use gizmos
- Use what you know about Unity to program a path following behaviour

Join the [class Facebook group](https://www.facebook.com/groups/153206655393239/)!

Anyone who implements their assignment using the [new Unity C# job system](https://www.youtube.com/watch?v=tGmnZdY5Y-E) will get a guaranteed first in the assignment.

Check out this video:

[![Video](http://img.youtube.com/vi/eAfpnWI5jEI/0.jpg)](http://www.youtube.com/watch?v=eAfpnWI5jEI)

The scene contains a path object with a path script attached. The children of the path object are the waypoints. The path script implements the method OnDrawGizmos to draw lines between the waypoints on the path and to draw a sphere at each of the waypoints. The blue box is following the path. Notice that it turns to face each of the waypoints smoothly and also that it banks as it turns. Today you can try and program this behaviour. Here are some steps you could follow:

Implement the Path script that contains a ```List<Vector3>``` containing the waypoints. The path script should populate this list from the attached children. It should also draw gizmos so that the path can be visualised. Check out the [Unity Gizmo documentation](https://docs.unity3d.com/ScriptReference/Gizmos.html). Gizmos only show up in the scene view in Unity and drawing gizmos will not impact on the performance of your game after you export it from Unity.

Create a path using the Path script you made.

Create the blue box and attach a script to it. You could call the script PathFollower. Add a a public field for the Path and drag the Path you made onto this in the Unity editor. Now write code to get the box to move from waypoint to waypoint on the path. When it reaches the last waypoint, it should loop back to the 0th waypoint again. There are lots of ways to do this. Here are some suggestions:

- Lerp from one point to the next and use LookAt
- Use a Rigidbody and generate forces to push the box from one waypoint to the next
- Use vectors
- Use [steering behaviours](https://www.red3d.com/cwr/steer/). This is the method we will explore in the class this week.

# Week 3 - Controllers & Physics
# Lecture
- We made the FPSControler and the ForceController. Get the code from the repo
- We also made the teniclce using rigid bodies and joints

## Lab
## Learning Outcomes

Today's lab is all about constructing physics objects in code.

If you already have the repo for the course, you can pull the latest version:

```
cd GP-2017-2018
git checkout master
git pull
```

You might need to commit your changes to the branch you were working on before switching back to the master branch:

```
git add .
git commit -m "My changes"
```

Open up the UnityIdioms project we were working on in the class last week and open scene3. This scene uses the FPS Camera controller we started work on in the class last week. Check out the code if you want to see how it works. You can move the camera using WASD and mouse look. You can steer the tank using IJKL and fire using space. Press Y to spawn a wall in front of the camera. Have a look the code in the class PhysicsSpawner to see how to do this

### Task 1
- Write a method ```CreateTower``` that creates a round tower of cubes in front of the tank in response to the U key. Do this using sin and cos to calculate the block positions. Try and make the tower stable.

#### Task 2
- Check out the video of my creatures:

    [![Video](http://img.youtube.com/vi/Ycv309jyzus/0.jpg)](http://www.youtube.com/watch?v=Ycv309jyzus)

- See if you can write a method called ```CreateTentacle``` that creates an articulated tentacle in code. You can use a squashed cube as a prefab, scaled to .1 on the Y axis and a hinge joint between the segments of the tentacle. Your method should take a parameter of the number of segments in the tentacle. Check out the Unity docs for [hinge joints](https://docs.unity3d.com/ScriptReference/HingeJoint.html).

# Week 2 - Unity Fundamentals

- Unity Fundamentals. Maks sure you know about:
    - GameObjects
    - GameComponents
    - Transforms, positions, quaternions
    - Lerping, Slerping and LookAt
    - Getting and adding gamecomponents programmatically
    - Awake, Start, Update
    - Instiantiating GameObjects from prefabs
    - Using materials an setting colours
    - The fundamentals of lighting
    - Using tags
    - Using Coroutines
    - Using Invoke

### Lab
### Learning Outcomes
- Creating game objects from prefabs
- Adding gamecomponents at runtime
- Using a coroutine    

Clone the repo for the course to get the tank game we started making in the class last week. Make a branch to store your changes to the code:
```
git clone http://github.com/skooter500/GE2-2017-2018
cd GP-2017-2018
git checkout -b lab1
```

The version here has a 3rd person camera that follows the tank and also the tank can fire bullets. You can use this as starter code for todays lab.

Make this in Unity:

[![Video](http://img.youtube.com/vi/wB4Ptbgwra0/0.jpg)](http://www.youtube.com/watch?v=wB4Ptbgwra0)
  
What is happening:

- Third person camera that follows the player tank, which is controlled with the wasd keys or joystick. Shoot with the a key or mouse click
- The game always tries to keep 5 target tanks around the player
- Tanks spawn every 2 seconds starting after 3 seconds into the game
- Bullets disappear after 5 seconds
- When a bullet hits a target tank, it should explode. After 3 seconds the pieces sink into the floor and a new tank will spawn    

## Week 1 - Introduction to Unity
- [Slides](https://drive.google.com/drive/folders/1HbqIE6_hwy0osMKDmEG5GgOAUeGy7NXV?usp=sharing)


