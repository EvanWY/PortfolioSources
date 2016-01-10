---
layout: portfolio_entry
title: Space Musketeer
timetext: Jul 2015 to Present
subtitle: My largest one-person game project
image: /img/spacemusketeer/cover.jpg
category: game
---

It is a multiplayer online shooting game, with top-down perspective and twin stick control. I design and programming, and find most of the art resources from Unity Asset Store. I try my best to make this game simple but high-quality.

###Overview

* **Role**: Designer, Programmer, Artist
* **Platform**: iOS 
* **Tools**: Unity 3D, MongoDB, Photoshop, etc.

###Video

<iframe width="510" height="340" src="http://www.youtube.com/embed/w" frameborder="0" allowfullscreen></iframe>

###Challenges

####Graphic Performance

* **Problem 1: Unable to reach 60 fps on target platform.** One of the most important goal of this game is to run smoothly and fast on mobile devices. All 3D games I have been involved was slow, so I gave myself this challenge.

* **Problem 2: Characters are not easily recognizable.** I use realistic character model at first. But the camera perspective made it hard to see all details on the model, and on the other hand, these details reduced game application speed.

* **Solution: Change character models and game object resources.** Actually, I change the art style of the entire game.

* **Result: The fps increased from 30 to 60. Characters became easily recognizable.** 

####Network

* **Problem: I used UNet as network solution, but it doesn't support multi-room.** UNet is a high level abstraction framework provided by Unity. It can solve complex problems with simple code. But each server could only have 1 battle field. It was difficult to build a game lobby with more than one rooms.

* **Solution: I built a new network toplogy model.** It has three types of nodes, Master Server Node, Slave Server Node and Client Node. Master server runs first, and slave server and client connect to it. When a client request to start a battle, the master server assign a slave server to set up the battle field, then tell the client to connect to it.

* **Result: It works well. It can also solve network latency problem.** This solution seperate rooms and lobby, so we can put slave servers all over the world. Then the network latency problem will be easily solved.

####Animation

* **Problem: Prepare animation resources for game program to control.** I bought all animations from Asset Store, and the animations were not yet binded with the character model skeletons. 

* **Solution: Use Animator Manager to blend animations and build state machine.** 

<div style="width:100%;"><div style="margin-left:50px;">
	Seperate the skeletons to upper body and lower body
	<div style="width:100%;"><div style="margin-left:15px;">
		<img src="/img/spacemusketeer/upperbody.jpg" align="middle" style="margin:5px 3px" width="200"/>
	</div></div>
	Build animation state machines for both upper body and lower body
	<div style="width:100%;"><div style="margin-left:15px;">
		<img src="/img/spacemusketeer/animfsmhand.jpg" align="middle" style="margin:5px 3px" width="400"/>
		<img src="/img/spacemusketeer/animfsmfoot.jpg" align="middle" style="margin:5px 3px" width="400"/>
	</div></div>
	Build animation blend tree for the aiming-walking state
	<div style="width:100%;"><div style="margin-left:15px;">
		<img src="/img/spacemusketeer/animfsmblendtree.jpg" align="middle" style="margin:5px 3px" width="400"/>
	</div></div>
</div></div>

* **Result: The character model was able to walk fluent when shooting to all directions.** 


####Physics

* **Problem: The game behaviour when players walk toward a wall have to be predicable.** I couldn't use collision algorithm in PhysX engine because the movement speed of the characters is discrete. If I use an unpredicable phyiscs engine, the synchronization will be complex and not smooth.

* **Solution: Impelemented a 2D collide-and-slide algorithm.** I didn't built the physics algorithm from scratch. I use the collision detection in PhysX and wrote the reaction algorithm. I use 2D physics to reduce performance impact.

<div style="width:100%;">
<div style="margin-left:50px;">
{% highlight C# %}
// calculate movement after collide and slide 
Vector2 slideMovementVec = CordinateUtil.calcMovementVec(slideSpdIdx, slideDirIdx, 
    Const.Movement.SPEED_IDX_NUM, Const.Movement.DIRECTION_IDX_NUM, 
    Time.deltaTime, playerDataSync.MaxMovementSpeed);

// if player hits something after collide and slide
if (playerProxy2D.calcHitArray(slideMovementVec, Const.Layer.WALK_OBSTACLE_MASK).Length > 0) {
    // set speed to 0
    // ...
}
else { 
    // move the player
    playerProxy2D.Move(slideMovementVec);
    // ...
}
{% endhighlight %}
</div>
</div>

* **Result: Accurate synchronization, high-performance, safety.** 2D physics ensure high-performance. And all movement logic is calculated in server, which ensure security.


