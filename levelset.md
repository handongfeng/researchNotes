http://profs.etsmtl.ca/hlombaert/levelset/

Level set method: Explanation

Here the basics of the level set method is explained, this is not an advanced tutorial. For quite some time, I was afraid of the level set method. I am still not sure why I first got scared. The level set method is just plain easy to understand: there is a surface, it intersects a plane, that gives us a contour and that's it. With image segmentation, the surface is updated with forces derived from the image. In this document, I follow this path: what is the problem, what was done to solve it and where the limitation was, and then the miracle solution with some pretty pictures.

Tracking interfaces

First, let us imagine water falling from the top of a hill. The goal is to track the water front while it's moving down the hill. The figure below shows a map of a V-shaped water fall with water motion shown in red.
Figure: Elevation map of a V-shaped water fall, black is on top, white is down the fall. Arrows show the Water motion.
Image motion
The question now is where is the water front at a given time t ?

Explicit contours

One way is to keep track of a few points, advance them in the normal direction to the front (the red arrows), and guess where the front ends up. The figure below shows a step of this technique.

Figure: Evolution of a set of points on the front at time t=0, and at time t=1
Image tracking
Evolving explicitly the front with a set of points may sound a good solution, but there are a couple of drawbacks that may want you to not use this technique.

With our last example of a V-shaped propagating front, corners can end up in an unknown state. The figure below illustrates such a state after one iteration.

Figure: Front evolution of a corner into an unknown state
Image tracking-v-shape
Also, if the front is expanding, the initial set of points may not be sufficient enough to define the evolving front. Points should be inserted or removed in the front when it is collapsing, and distance between points should be kept small enough for a smooth front. Such a mechanism could be troublesome during implementation.

Topology change requires further care during implementation (eg. split and merge of fronts). In our hill example, let us now imagine valleys and water moving from the top down into the valleys. The figure below illustrates such a scenario. The challenge is how to insert or remove points during a topology change.

Figure: Two valleys, black is on top, white is down the fall. (a) Initial water fronts on top of a valley, (b) spliting and merging of water fronts
Image tracking-valley-n-0	Image tracking-valley-n-50
(a) t=0	(b) t=50
Tracking the front with explicit contours is intuitive but can get troublesome during implementation.

Implicit contours

The idea of evolving a surface ($\phi$) instead of a front (C) was proposed, and the front is defined to be all points where the surface has no height ($\phi = 0$). The front is then defined implicitly as the zero level set $\phi = 0$. The figure below shows how the contour is extracted from the evolving surface.

Figure: Here the zero level set of the surface is a square
Image levelset-square
When the surface evolves, cups can appear, they can later narrow, or disappear. The zero level set shows contours spliting and merging as illustrated in the figure below where the plane at z=0 represents the map of our valley. The intersection of the surface $\phi$ with the plane creates the implicit contour. Merging and spliting are here handled naturally by the surface motion.

Figure: The evolving front in red is known by taking the zero level set of a surface $\phi$
Image levelset-n-50	Image levelset-n-52
(a) t=50, beginning of merging	(b) t=52, end of merging
Image levelset-n-90	Image levelset-n-120
(c) t=90, beginning of spliting	(d) t=120, end of spliting
No extra care is needed for topology change. This looks like an interesting idea, the question is now: what is the function $\phi$?

Level set equation

Let us look at the math behind this idea. A point  $\boldmath {x} = (x,y)$ belonging to a front evolves over time so that  $\boldmath {x}(t)$ is its position over time. At any time t, for each point  $\boldmath {x}(t)$ on the front the surface has by definition no height, thus:



\begin{eqnarray*}
\phi(\boldmath {x}(t),t) & = & 0
\end{eqnarray*}

The question still remains: what is the function  $\phi(\boldmath {x}(t),t)$? It can actually be anything we want as long as its zero level set gives us the contour. For example, a previous figure ([*]) showed an initial squared contour. The surface height is equal to the distance from (x,y) to the closest point on the contour, so that  $\phi(x,y,t=0) = \pm d$, with distance d positive outside the contour, and d negative inside it. So the initial $\phi$ can actually be any arbitrary function as long as its zero level set matches the initial contour

Given an initial $\phi$ at t=0, it would be possible to know $\phi$ at any time t with the motion equation  $\frac{\partial\phi}{\partial t}$. For that, the chain rule gives us:



\begin{eqnarray*}
\frac{\partial\phi\left(\boldmath {x}(t),t\right)}{\partial t}...
...\phi}{\partial \boldmath {x}(t)}\boldmath {x}_t + \phi_t & = & 0
\end{eqnarray*}

Here, recall that  $\frac{\partial\phi}{\partial \boldmath {x}} = \nabla\phi$. Also, the speed  $\boldmath {x}_t$ is given by a force F normal to the surface, so  $\boldmath {x}_t = F\left(\boldmath {x}(t)\right) \boldmath {n}$ where  $\boldmath {n} = \frac{\nabla\phi}{\vert\nabla\phi\vert}$. The previous motion equation can be rewriten with:



\begin{eqnarray*}
\phi_t + \nabla\phi \boldmath {x}_t & = & 0\\
\phi_t + \nabla...
...bla\phi\vert} & = & 0\\
\phi_t + F \vert\nabla\phi\vert & = & 0
\end{eqnarray*}

This last equation defines the motion of $\phi$. Given $\phi$ at time t=0, and its motion over time, it is now possible to know $\phi(x,y,t)$ at any time t by evolving the initial $\phi(x,y,t=0)$ over time. This answers our initial question, we now know what $\phi$ is.

An interesting feature that comes with $\phi$ is that it is possible to get the surface curvature with:



\begin{eqnarray*}
\kappa & = & \nabla \frac{\nabla\phi}{\vert\nabla\phi\vert} ...
...i_{xy}\phi_x\phi_y + \phi_{yy}\phi_x^2}{(\phi_x^2+\phi_y)^{1/2}}
\end{eqnarray*}

This could be useful to control the front smoothness.

Implementation

In the computer world, images have pixels and functions need to be discretized. That means $\phi_t$ will be evaluated at pixel (i,j) by  $\frac{\phi(i,j,t+\Delta t) - \phi(i,j,t)}{\Delta t}$. The gradient will be evaluated by a finite difference scheme, for example with:



\begin{eqnarray*}
\nabla^{+x}(i,j) = max\left[0,\Delta^{-x}\phi(i,j)\right]^2 + ...
...2 + min\left[0,\Delta^{-x}\phi(i,j)\right]^2, \textrm{when} F<0.
\end{eqnarray*}

Here,  $\Delta^{-x}\phi$ or  $\Delta^{+x}\phi$ is the left or the right side finite difference for a given point. This is illustrated in the figure below. The gradient is computed differently depending on the front direction and a finite difference scheme takes account of that.

Figure: left or right side finite difference (eg. how to evaluate $\vert\nabla\phi\vert$)
Image phi-difference
The previous motion equation now becomes:



\begin{eqnarray*}
\frac{\phi(i,j,t+\Delta t) - \phi(i,j,t)}{\Delta t} + max[F,0] \nabla^{+x}(i,j) + min[F,0] \nabla^{-x}(i,j) & = & 0
\end{eqnarray*}

From there, updating the surface $\phi(i,j)$ is done with:



\begin{eqnarray*}
\phi(i,j,t+\Delta t) & = & \phi(i,j,t) - \Delta t \left[ max[F,0] \nabla^{+x}(i,j) + min[F,0] \nabla^{-x}(i,j) \right]
\end{eqnarray*}

When computing the curvature, it depends only on the surface $\phi$, so central differences can be used. They are:



\begin{eqnarray*}
\phi_{xx}(i,j) &=& \left(\phi(i+1,j)-\phi(i,j)\right)-\left(\p...
...t)\\
\phi_y &=& \frac{1}{2}\left(\phi(i,j+1)-\phi(i,j-1)\right)
\end{eqnarray*}

The curvature is then computed numerically with:



\begin{eqnarray*}
\kappa(i,j) &=& \frac{\phi_{xx}\phi_y^2 - 2\phi_y\phi_x\phi_{x...
...- 2\phi_y\phi_x\phi_{xy} + \phi_{yy}\phi_x^2}{\phi_x^2+\phi_y^2}
\end{eqnarray*}

To keep a front smooth, high curvature should be penalized. That means, rather than decreasing $\phi$ in order to have the zero level set expanding, high curvature will reverse the front motion. Updating $\phi(i,j)$ with the curvature could be:



\begin{eqnarray*}
\phi(i,j,t+\Delta t) & = & \phi(i,j,t) - \Delta t \left[ max(F...
...) \nabla^{-x}(i,j) \right] + \Delta t \left[ \kappa(i,j) \right]
\end{eqnarray*}

Results

Given an initial arbitrary $\phi$, for example the distance transform of an initial contour, and a numerical scheme for the motion equation  $\frac{\partial\phi}{\partial t}$, it is possible to show some examples of contour evolution.

First a simple example of a drop of water exanding (F=1 everywhere) with an obstacle on its path (where F=0). The water front should be stopped by the obstacle and later be perturbed by the obstacle. This is shown in the figures below.

Figure: An initial circle expands where F=1 and is perturbed by an obstacle where F=0
Image circle-1	Image circle-2	Image circle-3
(a) initial contour	(b) stoped by an obstacle	(c) later perturbation
Then, a slightly more complex form illustrated below. The initial contour is still a circle, and the forces applied to it is positive (F=1) inside a form, and it is negative (F=-1) outside it. The contour should then be attracted into the form. Also notice the front split between the two regions. Here, the surface $\phi$ is also shown to explain how this split is formed.

Figure: An initial circle expands inside the form where F>0 and collapses outside the form (F<0), the corresponding $\phi$ surface is also shown with a plane intersecting at z=0
Image form-1	Image form-2	Image form-3
(a) initial contour	(b) contour split	(c) two new contours
After having shown how constant forces act on $\phi$, the next example shows a shape collapsing under its curvature. Here the force is equal to the curvature $\kappa$ of the surface $\phi$.

Figure: A form collapsing under its curvature
Image annealing-1	Image annealing-2	Image annealing-3
(a) initial contour	(b) intermediate step	(c) before desappearing
Now the fun begins with real pictures. Instead of using constant positive or negative forces, with or without curvature, the force is derived from an image. We can think of a contour stopped at the edges of an object. The force should then be high inside the object (we want the curve expanding inside an object), and it should be low close to the edges (we want the curve to stop at the edges). The gradient of an image shows where the edges are.

Figure: Image of a heart with its corresponding gradient
Image heart	Image heart-gradient	 
(a) image I	(b) its gradient $\vert\nabla I\vert$	 
The force to be used could be the inverse of the previous gradient image $\nabla I$, or it could be the gaussian of it:



\begin{eqnarray*}
F(i,j) &=& \frac{1}{1+\lambda \vert\nabla I(i,j)\vert}\\
F(i,j) &=& e^{-\frac{\vert\nabla I(i,j)\vert^2}{2\sigma^2}}
\end{eqnarray*}

Here $\lambda$ or $\sigma$ are parameters controlling how penalizing the edges should be. Below is an evolving contour using the inverse of the gradient image.

Figure: Contour evolution with a real image
Image heart-1	Image heart-2	Image heart-3
(a) initial contour	(b) intermediate step	(c) stopped by the edges
At this point, you should at least understand that level set method is all about evolving a surface instead of the real contour. That makes the beauty of this method. Level sets have applications in many fields. See google for further reading.

Bibliography

1
Osher S., Sethian J.A., Fronts Propagating with Curvature-Dependent Speed: Algorithms Based on Hamilton-Jacobi Formulations (Journal of Computational Physics, 79(1), page 12-49, 1988).
2
Kass M., Witkin A., Terzopoulos D., Snakes - Active Contour Models (International Journal of Computer Vision, 1(4), page 321-331, 1987)
