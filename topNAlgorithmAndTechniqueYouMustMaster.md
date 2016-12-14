For any scientist and engineer in advanced medical imaging computing field.
You should master below algorithm and technique. Maninly for software point of view

1. SVM: You should not only know how to use it, you should can derivate the dual formulation of SVM. 
2. Divide and conquer thchnique: for example quick sort, binary search
3. DP: DP could be very broadly range. But you should master some commonly used DP algorithms. such as clibe stairs, shorest path for an image along each rows, etc.
4. Graph algorithms: shorest path, DFS, BFS, topological sort, regions growing, dilate and erode
5. min/max heap, together with graph shorest path
6. Tree structure. k-d tree
7. Regression
8. K-NN: nearest neighbor search
9. Commonly used data structure: vector, map, set pair, std::sort, boost smart pointer,
10. Coordinate system and conversion between different spaces. [1] volume voxel space; [2] physical space (world space); [3] Anatomical space; Details are here. https://crab6789.wordpress.com/2014/10/13/medical-image-volume-coordinate-system/
11. Point matching. Given two points set, compute the global Rotation and Translation. This is a very usefult method can quick computing the optimal rigid motion between two sets. We can further revise to use Ransac to make it more robust. A details solutions is here https://igl.ethz.ch/projects/ARAP/svd_rot.pdf. Here I only list the compouting steps.* The goal is to find R,t that minimize $$\sum_{i=1}^{n}w_i||Rp_i+t-q_i||^2$$
[1] Compute the weighted centroids of two point set p and q:
$$\overline{p} = \frac{\sum_{i=1}^{n} w_ip_i}{\sum_{i=1}^{n}w_i}, \overline{q} = \frac{\sum_{i=1}^{n} w_iq_i}{\sum_{i=1}^{n}w_i}$$
[2] Compute the centered vectors:
$$ x_i = p_i-\overline{p}, y_i= q_i-\overline{q}, i= 1,...,n$$
[3] Compute the $d \times d$ matrix $S$ 
$$S =XWY^T$$
$X$ is the $d \times n$ matrix and each columen is one point. $W = diag(w_1,w_2,...,w_n)$
[4] Compute the SVD $S=U \Sigma V^T$. So
$$R = VU^T$$
[5] translations is $$ t = \overline{q}-R\overline{p}$$
