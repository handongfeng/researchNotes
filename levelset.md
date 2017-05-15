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









http://profs.etsmtl.ca/hlombaert/region/
Region based segmentation with levelsets

Edge based segmentations are intuitive since edges usually show an object boundary. However in some or many cases, edges are not sharp or are simply not there. Region based segmentations is a solution to this problem. Here is an example using a level set framework.

Edge based segmentation

When trying to isolate an object, its edges are good hints for the object boundary. That is why typical edge based objective functions make use of the gradient $\nabla I$. In a level set framework, high gradients should stop motion, and low gradients should allow the level set motion. For example, motion can be the inverse of the gradient, or it can be its gaussian:


\begin{eqnarray*}
F & = & \frac{1}{1+\lambda\vert\nabla I\vert}, \textrm{or}\\
F & = & e^{-\frac{\vert\nabla I\vert^2}{2\sigma^2}}
\end{eqnarray*}

Here, the parameter  $\left.\lambda\right.$ or  $\left.\sigma\right.$ defines the range of penalizing gradient values: if  $\left.\lambda\right.$ is high or if  $\left.\sigma\right.$ is low then only the strongest gradients will stop motion.

When an object boundary is obvious (see figure below), these objective functions work well.

Figure: Level sets using  $F=\left.e^{-\frac{\vert\nabla I\vert^2}{2\sigma^2}}\right.$
Image knee-initial	Image knee-final
(a) Initial contour	(b) Final contour
However if the object boundary is not well defined, a weak gradient will not stop the contour. Changing the parameter  $\left.\lambda\right.$ or  $\left.\sigma\right.$ will possibly slow down the level set motion but it will not prevent the contour to go beyond the boundary. There will be leakage in the neighboring regions as shown in the figure below.

Figure: Level sets using  $F=e^{-\frac{\vert\nabla I\vert^2}{2\sigma^2}}$
Image vessel-initial	Image vessel-intermediate	Image vessel-leakage
(a) Initial contour	(b) Intermediate step	(c) Leakage
Now, simple synthetic examples will be used in order to illustrate the leakage, and this will help find a solution. The figure below shows an ideal situation with a bright square on a dark background. Its gradient shows a clear boundary. The level set will be stopped there.

Figure: In ideal condition, edge based segmentations work
Image square0	Image square0-gradient	Image square0-final
(a) Ideal solution	(b) Clear gradient	(c) Contour matches
To get closer to real situations, a gaussian noise has been added to the last image. It is still possible to see a bright square on a dark background, but the image gradient does not show a clear single boundary any more. Some spots on the boundary have a weak gradient. This is where leakage will happen as shown in the figure below:

Figure 4: In real conditions, edge based segmentation leaks
Image square1	Image square1-gradient	Image square1-final
(a) Noisy image	(b) Weak gradient	(c) Contour leaks
Filtering the image may help, but there is a chance the edges get blurred and leakage will then still occur. In any case, the image is modified and information may be lost. Below is an example of bluring with a gaussian filter, the gradient looks better, but there is still a leakage.

Figure 5: In real conditions, edge based segmentation leaks
Image square2	Image square2-gradient	Image square2-final
(a) Noisy image	(b) Weak gradient	(c) Contour leaks
Region based

The segmentation problem can be tackled differently: instead of looking for boundaries, we look for regions. That means the objective function should enhance regions rather than boundaries. In a level set framework, two regions are easily known with  $\left.\phi<0\right.$, inside the contour, and with  $\left.\phi>0\right.$, outside the contour. The objective function can measure the homogeneity of both regions.

One way of measuring the homogeneity of a region is with the least squared mean. For this, the mean of pixel intensities of each region is needed:



\begin{eqnarray*}
\mu_0 & = & \frac{1}{n_0}\sum_{\phi<0} I(i,j)\\
\mu_1 & = & \frac{1}{n_1}\sum_{\phi>0} I(i,j)
\end{eqnarray*}

with here n0 the number of pixels inside the contour, n1 the number of pixels outside the contour. The least squared mean is then:



\begin{eqnarray*}
F & = & \left.\sum_{\phi<0}\right. \left(I(i,j)-\mu_0\right)^2 + \left.\sum_{\phi>0}\right. \left(I(i,j)-\mu_1\right)^2
\end{eqnarray*}

With this objective function, if a pixel has an intensity too different than the mean of region where he is currently classified, it will be penalized. If its intensity is closer to the mean of the other region, the level sets will eventually classify this pixel in the other region. The example below shows how this objective function behaves in the previous problem.

Figure 6: Region based segmentation with the least squared mean
Image square3-1	Image square3-2	Image square3-3
(a) Noisy image	(b) Intermadiatekstep	(c) The homogeneous regions
As the level sets evolve, pixels get classified in the region where the mean is closer to the pixel intensity. However, the previous object function is always positive, that means the level sets cannot move back or undo a decision. If a pixel is wrongly classified within an initial contour, it will remain misclassified. This is shown in the figure below.

Figure: Misclassification when  $\left.F>0\right.$
Image square4-1	Image square4-2	Image square4-3
(a) Initial contour	(b) Contour only grows outward	(c) Final contour
From the previous example, the surface  $\left.\phi\right.$ is shown, the force always positive makes the surface only move downward. The contour can then only grow outward. In such a situation, the dark square inside the initial contour cannot be classified as a background. For that to happen, the surface would have to move upward.

Instead of using the squared mean, the plain difference to the mean would allow negative forces.



\begin{eqnarray*}
F & = & \left.\sum_{\phi<0}\right. \left(I(i,j)-\mu_0\right) + \left.\sum_{\phi>0}\right. \left(I(i,j)-\mu_1\right)
\end{eqnarray*}

If the dark square gets incorrectly selected, the first term of the objective function is negative, and the surface will move upward as shown in the figure below.

Figure 8: Inner contours in region based segmentation
Image square5-1	Image square5-2	Image square5-3
(a) Initial contour	(b) Surface can now move upward	(c) Final contour
The last objective function will always select brighter region. So if the initial contour is placed in a dark area, the surface will evolve so that  $\left.\phi<0\right.$ will become the bright region as shown in the figure below.

Figure 9: Wrong initialisation in region based segmentation
Image square6-1	Image square6-2	Image square6-3
(a) Initial contour in dark area	(b) Intermediate step	(c) Final contour
The last objective function is robust to noise as long as the noise does not change the homogeneity of each region: bright regions with noise will still remain bright, and dark regions with noise will still remain dark. The example below shows the last image with noise.

Figure 10: Region based segmentation with noise
Image square7-1	Image square7-2	Image square7-3
(a) Initial contour	(b) Intermediate step	(c) Final contour
Every point on the surface is independant from its neighboring points and can have a different motion. That is why noise creates a spiky surface. Adding tension to the surface will make each point dependant from their neighbors, and it will prevent spikes. Adding a curvature term  $\left.\epsilon\kappa\right.$ to the objective function gives:



\begin{eqnarray*}
F & = & \sum_{\phi<0} \left(I(i,j)-\mu_0\right) + \sum_{\phi>0} \left(I(i,j)-\mu_1\right) + \epsilon\kappa
\end{eqnarray*}

However if this term is important, sharp corners (as in a square) get lost. The example below shows the same situation as before with a little viscosity added, corners get a little bit rounded.

Figure 11: Using surface tension in a region based segmentation with noise
Image square8-1	Image square8-2	Image square8-3
(a) Initial contour	(b) Intermediate step	(c) Final contour
With this last result, the leaking problem has been solved. Notice also that unconnected contours, in particular inner contours are now handled.

Results

Using the previous objective function, it is now possible to segment the vessel image as shown below.

Figure 12: Vessel image using a region based objective function
Image vessel1-1	Image vessel1-2	Image vessel1-3
(a) Initial contour	(b) Intermediate step	(c) Final contour
This picture shows that region based segmentation are not prune to leakage. However, the boundary may not be exactly where one think it would be. A mixture of edge based terms and region based terms in the objective function might be a good combination.

