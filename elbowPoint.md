questions
Finding the best trade-off point on a curve
As shown in the pic, we want to get a point area
Solved in http://stackoverflow.com/questions/2018178/finding-the-best-trade-off-point-on-a-curve/2022348#2022348

The basic idea is:
draw a line from the first to the last point of the curve and then find the data point that is farthest away from that line.

an example image is shown here

![alt text][logo]
[logo]: https://github.com/handongfeng/researchNotes/blob/master/elbowSolution.png "solution illustration"



Solution
http://mathworld.wolfram.com/Point-LineDistance2-Dimensional.html

[1] given two point (x1,y1) (x2,y2) define a line L, compute the distance from point (x0,y0) to L.
$\d = | v,r | = \frac{ | (x_2-x_1)(y_1-y_0) - (x_1-x_0)(y_2-y_1) | }{ \sqrt{  (x_2-x_1)^2 + (y_2-y_1)^2} }$
