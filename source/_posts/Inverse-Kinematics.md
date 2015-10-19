title: Inverse Kinematics
date: 2015-10-17 20:33:53
tags:
- Inverse Kinematics
- COMP4411
- Computer Graphics
- OpenGL
- C++
---
Background
----------
The second project of COMP4411: Computer Graphics, is Modeler. The basic requirements are about coming up with a model you like and draw it using OpenGL. That's not fun. However I looked at the last bonus and was amazed at how many bells it evaluates: $8+4$ bells!

Gradient descent
----------------
The first idea that came into my mind was Gradient descent! That's also the basic idea in any machine learning problems. Here in our case, we'll be trying to minimize a function that maps from the angle combination space to the distance space. I decided to calculate the derivatives numerically. So here's what I need: a cost funcion.

To be able to calculate the cost given a vector of angles of joints, I'll have to calculate the position of a certain object.

After some research, I found the feedback mode of OpenGL could come in handy. However it turned out to only produce the rasterized coordinate in screen space.

So I have to calculate the coordinate by hand. I added a bunch of matrices to our Model, and created a function to recursively walk the limb up to the torsal and multiply the transformation matrices along the way to give a final map.

After this is done, I'll be able to calculate the gradient: remember the state vector, add/subtract a pre-defined delta value to a given element, then calculate the new cost. Finally divide the difference in cost by the delta timed by two. And every step I subtract the state by a learning rate times gradient vector.

It worked!

plus momentum
------------------------------
Gradient descent worked , but not greatly. There's oscillation when the state comes pretty close to the local minimum it doesn't always converge. That's a natural drawback for gradient descent.

[Image of gradient descent jitter]

So next I added momentum to the gradient descent. The major difference is that each time the gradient is added to a momentum first, then the momentum is added to the state. So the direction of previous iterations is kept by the momentum vector, and thus eliminating the oscillation. As a result the algorithm could converge much faster and more nicely.

plus random initial values
--------------------------
One major headache occurred here. If the leg of our model is straight down at the beginning, then when we require it to move straight up it would be stuck in the original place.

[Image of stuck local minimum]

That's because of the local minimum. Since our target is located right above the foot, any motion of any angle would cause it to briefly diverge from the seemingly minimum cost position.

It's a difficult one to fix because local minimum is the nature of gradient descent. We had to figure out some way to break out of the local minimum. What occurred to me was random initialization.

So I would random initialize the state to a neighboring state, and then do the gradient descent. But the convergence became very slow.

I need some way else.

Simulated Annealing
-------------------
Yeah that's the final solution we were looking for. Simulated annealing is the process inspired by the annealing of forgery of weapons. When the cooling rate of metal is controlled, hot atoms that are vibrating in an extremely high rate will have enough time to find a niche where the energy is lowest for it, thus allowing the whole crystal to stablize in an overally lower-energy state.

[the gif of simulated annealing]

And in our case, we'll simulate a temperature and lower the temperature would give rise to lower possibility of jumping to a neighboring area of higher cost area. Here the key that differentiate simulated annealing from gradient descent is its allowance for moving to a higher-cost state.

Given the temperature T, the original cost e and new cost e', the probability for switch is 1 if e'<e, which is sensible, and exp(-(e' - e)/T) if e'>e. We can see that when T is high, the value approaches 1. When T is small however, it releases -(e' - e) into farther left of the number axis, lowering the value outputed by the exponential funciton.

But we could not use Simulated Annealing alone because the direction for its neighborhood search is unpredictable and converging would require too long a time due to the slow cooling.

So the idea is, Simulated Annealing is used to only replace the random initialization to give a more purposed randomization, and then the control is handed over to gradient control and the final state is determined there.

THe energy consumption
----------------------
Apart form the goal-oriented cost minimization, another metric is added to the overall cost function, which is the divergence from the original post at the beginning of the Inverse Kinematics optimization. Doing so allows us to hold the joints back at their original posts when we are not moving any joints related to them. Such problem won't exist if there's no randomization in the first place, though. It's a tradeoff.

Analytical solution
-------------------
Professor suggested that a possible analytical solution can be achieved. The formula for 2-norm is basically a quadratic equation in matrix form, and there's a way to find its analytical solution. However that would require using symbolic software like Mathematica which I unfortunately am not so familiar with. And since our method is working fine enough, we've decided to leave for another day.

*updating...*
