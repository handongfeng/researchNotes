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

19. 回到数学上，不相关只是说两个变量不线性相关，协方差为0。协方差反映什么？如果两个随机变量，一个变大，另一个也跟着它变大，那么协方差大于0；如果一个变大另一个变小，那么协方差小于0。如果统计学上算下来，一个就不跟着另一个变化，那么协方差等于0——它们俩不相关。
20. matconvnet mxnet
21. Max–min inequality. It is used in primal-dual of SVM. https://en.wikipedia.org/wiki/Max%E2%80%93min_inequality
22. Using conda to install different python version. https://conda.io/docs/py2or3.html#id2
23. 胡适的代表作《中国哲学史大纲》、《白话文学史》皆有始无终，只有上卷而没有下卷，以致被黄侃讥讽为缺失“下面”一截的太监，成了将没有完稿的小说、论著称为“太监”的起源。From wiki https://zh.wikipedia.org/wiki/%E8%83%A1%E9%81%A9
24. 数据江湖，风起云涌。各路英豪，群雄逐鹿。

这是一个数据科学最好的时代，也是数据江湖最乱的时代。

那么在这么一个特殊的江湖里面浪，有什么兵器是值得我们去关注的呢？这篇文章列举
了一些常用方法（刀剑），并不涵盖工具与平台。就先让我们一起去看看这个排名不分
先后左右的兵器谱。

数据科学家Vincent Granville博士发表博文列举了数据科学家常用的45种技术。这是
个很适合初学者去逐个了解的列表。当然，这并不代表数据科学（统计学）的全部。虽
然他并没有提出自己的详细总结，但是有志于学习数据科学的同学不妨初步有个印象，
有不太熟悉的topic可以进一步去了解一下。另外我在后面也补充了我认为也值得学习
的领域，很多人都会在日常的数据实践中用到。多学有益于身心健康。

首先需要说明的是，这些技术只是大概涵盖了大部分数据科学家以及相关领域的实践者
日常用的方法。这一般意味着他们或使用第三方的解决方案（比如R和Python里面提供
的相关package），或者自己需要调整或者重新设计合适的工具。    

废话少说，列举如下：

    1. 线性回归     Linear Regression
    2. 逻辑回归     Logistic Regression 
    3. 刀切回归     Jackknife Regression *
    4. 密度估计     Density Estimation 
    5. 置信区间     Confidence Interval 
    6. 假设检验     Test of Hypotheses 
    7. 模式识别     Pattern Recognition 
    8. 聚类(或者无监督学习)        Clustering - (aka Unsupervised Learning)
    9. 有监督学习    Supervised Learning 
    10. 时间序列分析    Time Series 
    11. 决策树    Decision Trees 
    12. 随机数    Random Numbers 
    13. 蒙特卡洛模拟    Monte-Carlo Simulation 
    14. 贝叶斯统计    Bayesian Statistics 
    15. 朴素贝叶斯    Naive Bayes 
    16. 主成分分析    Principal Component Analysis - (PCA)
    17. 联合学习方法    Ensembles 
    18. 神经网络        Neural Networks 
    19. 支持向量机    Support Vector Machine - (SVM)
    20. 最近邻方法    Nearest Neighbors - (k-NN)
    21. 特征选择(变量削减)    Feature Selection - (aka Variable Reduction)
    22. 指数化（编目化）Indexation / Cataloguing *
    23. 空间统计建模（时空统计）(Geo-) Spatial Modeling 
    24. 推荐引擎    Recommendation Engine *
    25. 搜索引擎    Search Engine *
    26. 归因模型    Attribution Modeling *
    27. 协同过滤    Collaborative Filtering *
    28. 规则系统    Rule System 
    29. 连锁分析    Linkage Analysis 
    30. 关联规则    Association Rules 
    31. 打分引擎    Scoring Engine 
    32. 分割（特指数据分割）Segmentation 
    33. 预测建模    Predictive Modeling 
    34. 图数据分析    Graphs 
    35. 深度学习        Deep Learning 
    36. 博弈论        Game Theory 
    37. 数据填充        Imputation 
    38. 生存分析        Survival Analysis 
    39. 统计套利        statistical Arbitrage 
    40. 推举建模        Lift Modeling 
    41. 产量优化        Yield Optimization
    42. 交叉验证        Cross-Validation
    43. 模型拟合        Model Fitting
    44. 关联算法那    Relevancy Algorithm *
    45. 实验设计        Experimental Design

以上是Granville博士的观点。

以我自己的经验，一般数据科学家并不会涉及这上面的所有方法，而且很多重要的技术
也没有在上面出现，比如在我的观点中，一下对于日常的数据战场特别有帮助的武功

a 凸优化（convex analysis）
b 组合优化（combinatorial optimization）
c 半监督学习 （semi-supervised learning）
d 采样（sampling）
e 强化学习 （reinforcement learning）
f 自组织映射 （self-organized map)
g 独立成分分析 （ICA）
h 降维    （dimensioin reduction）
i 最大似然估计 （MLE）
j 通用核方法 （general kernal method）
k 模型选择 （model selection）
l 样条方法 （spline method）
m 正则化 （regularization)
n 归一化 （normalization）
o 经验贝叶斯 （empirical bayes）
p EM算法 （EM algorithm）
q 变分发 （variational method）
r 图模型    （graphical models）
s 可视化    （visualization）
t 高斯混合模型    （Gaussian mixture models）
u 异常检测（abnormity/outlier detection）
v 方差分析 （ANOVA）
w 遗传算法 （genetic algorithm） 
x 算法表现评估 （algorithm performance evaluation）
y 计算机视觉主流方法 （computer vision）
z 自然语言处理主流方法 （Natural language processing)

天下功夫，万变不离其宗。所有的套路与武器，都是为了在数据的海洋里面寻找到目标
问题的解药。方法无所谓高低，招数无所谓贵贱。能够达成你的目标的，就是最适合你
当下的武器。

希望这些对大家的数据实践有所帮助。



The sampled time waveform input to an FFT determines the computed spectrum. If an arbitrary signal is sampled at a rate equal to fs over an acquisition time T, N samples are acquired. Compute T with the following equation:

where

T is the acquisition time

N is the number of samples acquired

fs is the sampling frequency

Compute N with the following equation:

N = T · fs
