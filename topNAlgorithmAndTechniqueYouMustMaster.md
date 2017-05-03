For any scientist and engineer in advanced medical imaging computing field.
You should master below algorithm and technique. Maninly for software point of view

1. SVM: You should not only know how to use it, you should can derivate the dual formulation of SVM. 
2. Divide and conquer thchnique: for example quick sort, binary search
3. DP: DP could be very broadly range. But you should master some commonly used DP algorithms. such as clibe stairs, shorest path for an image along each rows, etc.
4. Graph algorithms: shorest path, dijkstra algorithm, DFS, BFS, topological sort, regions growing, dilate and erode
5. min/max heap, together with graph shorest path
6. Tree structure. k-d tree
    * for a full binary tree [ $n=2h+1$, $2^{h+1}-1$], where h is the height of the tree, where root is has height of 0
    * a perfect full binary tree $l= 2^h$  and $n = 2^{h+1}-1$.  $l$ is the number of leaf nodes
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
$$R = V  \begin{pmatrix}1 & 0 & \cdots & 0 \ 0 & 1 & \cdots & 0 \ \vdots & \vdots & \ddots & \vdots \0 & 0 & \cdots & det(VU^T) \end{pmatrix} U^T$$
[5] translations is $$ t = \overline{q}-R\overline{p}$$

$$
\left[\begin{array}
{rrr}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{array}\right]
$$

12. Smart pointer. 
...  Use unique_ptr when you want a single pointer to an object that will be reclaimed when that single pointer is destroyed.
...  Use shared_ptr when you want multiple pointers to the same resource.
shared_ptr<T> sp1(new T(L"Lady Gaga", L"Just Dance"));
... Rules: try to use smart pointer instead of new and delete! 
... example usage is here: https://msdn.microsoft.com/en-us/library/hh279669.aspx
13. Power of 2
![Image of Powerof2Table](https://github.com/handongfeng/researchNotes/blob/master/powerof2.jpg)
| power of 2 |  value            | arrox.     | byte to M GB||
|    7       |  128              |            |             ||
|    8       |  256              |            |             ||
|    10      |  1024             | 1 thousand | 1k          ||
|    16      |  65536            |            |  64K        ||
|    20      |  1,048576         | 1 million  |  1MB        ||
|    30      |  1,07374741824    | 1 billion  |  1GB        ||
|    32      | 4,294,967,296     |            |  4GB        ||
|    40      | 1,099,511,627,776 | 1 trillion |  1TB        ||

14. Common functions figure. log(x), exp(x) etc.
15. Numeric stability. When the number is too large or too smalle, take care of the numeric stability. Overflow and underflow
16. c++ 11. http://shaoyuan1943.github.io/2014/06/10/C++-11/
17. http://blog.csdn.net/jacketinsysu/article/details/52434669
#include <iostream>
#include <sstream>
#include <iomanip>
using namespace std;

double round(double number, unsigned int bits) {
    stringstream ss;
    ss << fixed << setprecision(bits) << number;
    ss >> number;
    return number;
}

int main() {
    double number = 3.1415926535897932;
    cout << fixed << showpoint << setprecision(15);
    cout << "一开始number = " << number << endl;

    for (int i = 0; i < 15; ++i) {
        cout << "number保留" << i << "位小数后为: " 
            << round(number, i) << endl;
    }
    
18. https://www.coder-note.com/questions/17333/most-effective-way-for-float-and-double-comparison

  bool approximatelyEqual(float a, float b, float epsilon)
{
    return fabs(a - b) <= ( (fabs(a) < fabs(b) ? fabs(b) : fabs(a)) * epsilon);
}

bool essentiallyEqual(float a, float b, float epsilon)
{
    return fabs(a - b) <= ( (fabs(a) > fabs(b) ? fabs(b) : fabs(a)) * epsilon);
}

bool definitelyGreaterThan(float a, float b, float epsilon)
{
    return (a - b) > ( (fabs(a) < fabs(b) ? fabs(b) : fabs(a)) * epsilon);
}

bool definitelyLessThan(float a, float b, float epsilon)
{
    return (b - a) > ( (fabs(a) < fabs(b) ? fabs(b) : fabs(a)) * epsilon);
} 
