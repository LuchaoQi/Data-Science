[TOC]



PCA

>Reduce dimension using the eigenpairs of covariance matrix







### [What are principal component scores?](https://stats.stackexchange.com/questions/222/what-are-principal-component-scores)

https://stats.stackexchange.com/a/282/272857



In NHANES project, our PC scores can be calculated

```R
load('analyticData.rda')

subject_scores = MINdata %>% select(-SEQN)
PC_scores = scale(subject_scores, pca_model$center, pca_model$scale) %*% pca_model$rotation

# or 
# prcomp(DF, scale = FALSE)$x
pca_model$x
```





### [How to project a new vector onto PCA space?](https://stats.stackexchange.com/questions/2592/how-to-project-a-new-vector-onto-pca-space)



```R
# perform principal components analysis
pca <- prcomp(data) 

# project new data onto the PCA space
scale(newdata, pca$center, pca$scale) %*% pca$rotation 
```





### Loadings

https://stats.stackexchange.com/a/143949



$$
\text { Loadings }=\text { Eigenvectors } \cdot \sqrt{\text { Eigenvalues }}
$$



raw data %*% loadings = rotated data

```R
loadings = prcomp(d)$rotation[,'PC1']
PC1_scores = d %*% ( prcomp(d)$rotation[,'PC1']) # d %*% loadings
PC1_scores = prcomp(d)$x[,'PC1']
```









### [PCA on a categorical variable?](https://piazza.com/class/k1zer6tg6s04bb?cid=443)

> Unfortunately, no. PCA is really only appropriate for real-valued features. Even ordinal features (categories with order, e.g. first, second, third, ...) shouldn't use PCA because there is a "distance" implied. Only use PCA for xi∈R.
>
> 
>
> They mention correspondence analysis, which is a method I haven't looked into myself, but here's a link to the paper information (it's from 1935): https://www.cambridge.org/core/journals/mathematical-proceedings-of-the-cambridge-philosophical-society/article/connection-between-correlation-and-contingency/D3A75249B56AF5DDC436938F1B6EABD1
>
> They also mention multi-factor analysis: https://www.sciencedirect.com/science/article/pii/016794739490135X?via%3Dihub

https://stackoverflow.com/a/40801571/11964524

> I disagree with the others.
>
> While **you can use PCA on binary data** (e.g. one-hot encoded data) that does not mean it is a good thing, or it will work very well.
>
> PCA is desinged for *continuous* variables. It tries to minimize variance (=squared deviations). The concept of squared deviations breaks down when you have binary variables.
>
> So yes, you can use PCA. And yes, you get an output. It even is a least-squared output - it's not as if PCA would segfault on such data. It works, but it is just much less *meaningful* than you'd want it to be; and supposedly less meaningful than e.g. frequent pattern mining.



### Does mean centering or feature scaling affect a Principal Component Analysis?

https://stats.stackexchange.com/a/69159/272857

| Before Normalization                     | After Normalization                      |
| ---------------------------------------- | ---------------------------------------- |
| ![](https://i.stack.imgur.com/lS3wA.png) | ![](https://i.stack.imgur.com/rqYHf.png) |

https://sebastianraschka.com/faq/docs/pca-scaling.html







## Intuition: why we use covariance matrix and why we use eigenpairs?

**Definition of covariance matrix**



https://www.bilibili.com/video/BV1aE411o7qd?p=23

https://en.wikipedia.org/wiki/Covariance_matrix#Definition



![](https://wikimedia.org/api/rest_v1/media/math/render/svg/83bec85f5e2cab5d3406677dd806e554a442331f)



**Projected distance**

$\vec{a}*\vec{b} = \vert \vec{a}\vert * \vert \vec{b}\vert * cos\theta$

which is equal to  $\vec{a}^T * \vec{b}$ 





Note we denote $S$ as covariance matrix

**After centering the data, we want to maximize the projected variance**

Assuming the unit projected direction/vector is $u_1$




$$
\begin{aligned} J &=\frac{1}{N} \sum_{i=1}^{N}\left(\left(x_{i}-\bar{x}\right)^{\top} u_{1}\right)^{2} \quad \text { s.t. }\left|u_{1}\right|=1 \text { s.t. } u_{1}^{\top} u_{1}=1 \\ &=\sum_{i=1}^{N} \frac{1}{n} u_{1}^{\top}\left(x_{i}-\bar{x}\right) \cdot\left(x_{i}-\bar{x}\right)^{\top} u_{1} \\ &=u_{1}^{\top}\left(\sum_{i=1}^{N} \frac{1}{n}\left(x_{i}-\bar{x}\right) \cdot\left(x_{i}-\bar{x}\right)^{\top}\right) u_{1} \\ &=u_{i}^{\top} \cdot s \cdot u_{1} \end{aligned}
$$




so we need to solve an optimization problem


$$
\left\{\begin{array}{l}{\hat{u}_{1}=\arg \max u_{1}^{\top} \cdot s \cdot u_{1}} \\ {\text { s.t. } u_{1}^{\top} u=1}\end{array}\right.
$$


use Lagrange equation

$$
\begin{array}{c}{\mathcal{L}\left(u_{1}, \lambda\right)=u_{1}^{\top} s u_{1}+\lambda\left(1-u_{1}^{\top} u\right)} \\ {\frac{\partial \mathcal{L}}{\partial u_{1}}=2 s \cdot u_{1}-\lambda \cdot 2 u_{1}=0} \\ {S u_{1}=\lambda u_{1}}\end{array}
$$
DONE! $u_1$ is eigenvector and $\lambda$ is eigenvalue





## Example



centering the data before PCA



e.g. we have five 2-D data points after centering



$$
A = \begin{pmatrix}
-1 & -1 & 0 & 2 & 0\\
-2 & 0 & 0 & 1 & 1
\end{pmatrix}
$$


then we calculate covariance matrix


$$
\begin{align*}
C = \frac{1}{5} * \begin{pmatrix}-1 & -1 & 0 & 2 & 0 \\
-2 & 0  & 0 & 1 & 1\end{pmatrix} * \begin{pmatrix}-1 & -2 \\ -1 & 0 \\ 0 & 0 \\ 2 & 1 \\ 0 & 1\end{pmatrix} = \begin{pmatrix} \frac{6}{5} & \frac{4}{5} \\ \frac{4}{5} & \frac{6}{5}\end{pmatrix}
\end{align*}
$$



eigenvalues/eigenvectors


$$
\begin{array}{c}{\lambda_{1}=2, \lambda_{2}=2 / 5} \\ {c_{1}\left(\begin{array}{c}{1} \\ {1}\end{array}\right), c_{2}\left(\begin{array}{c}{-1} \\ {1}\end{array}\right)}\end{array}
$$


sort the eigenvectors based on eigenvalues and cast them into a new matrix so eigenvectors in row
$$
P=\left(\begin{array}{cc}{1 } & {1 } \\ {-1} & {1 }\end{array}\right)
$$


standardized eigenvector matrix


$$
P=\left(\begin{array}{cc}{1 / \sqrt{2}} & {1 / \sqrt{2}} \\ {-1 / \sqrt{2}} & {1 / \sqrt{2}}\end{array}\right)
$$


which has the following feature


$$
P C P^{\top}=\left(\begin{array}{cc}{1 / \sqrt{2}} & {1 / \sqrt{2}} \\ {-1 / \sqrt{2}} & {1 / \sqrt{2}}\end{array}\right)\left(\begin{array}{cc}{6 / 5} & {4 / 5} \\ {4 / 5} & {6 / 5}\end{array}\right)\left(\begin{array}{cc}{1 / \sqrt{2}} & {-1 / \sqrt{2}} \\ {1 / \sqrt{2}} & {1 / \sqrt{2}}\end{array}\right)=\left(\begin{array}{cc}{2} & {0} \\ {0} & {2 / 5}\end{array}\right)
$$

use the first line (`first eigenvector/largest eigenvalue`)

project the original data points into new coordinate
$$
Y=\left(\begin{array}{cc}{1 / \sqrt{2}} & {1 / \sqrt{2}}\end{array}\right)\left(\begin{array}{ccccc}{-1} & {-1} & {0} & {2} & {0} \\ {-2} & {0} & {0} & {1} & {1}\end{array}\right)=\left(\begin{array}{cccc}{-3 / \sqrt{2}} & {-1 / \sqrt{2}} & {0} & {3 / \sqrt{2}} & {1 / \sqrt{2}}\end{array}\right)
$$
P.S. 我发现很多网上的例子都不自己算一下就直接复制 像上面计算Y简单的矩阵运算都是错的

DONE! the projected 1-D points are shown as below



![](https://i.loli.net/2020/06/05/Zhzjl2tHmn43vEU.png)







## Code

### R

R code for the above example

```R
# all features on top
d = matrix(c(-1,-1,0,2,0,-2,0,-0,1,1),ncol = 2)
d
pca_res = prcomp(d)
pca_res
```

```R
     [,1] [,2]
[1,]   -1   -2
[2,]   -1    0
[3,]    0    0
[4,]    2    1
[5,]    0    1
Standard deviations (1, .., p=2):
[1] 1.5811388 0.7071068

Rotation (n x k) = (2 x 2):
           PC1        PC2
[1,] 0.7071068  0.7071068
[2,] 0.7071068 -0.7071068
```



`Rotation` or `loadings` in princomp() is the matrix of **standardized** variable loadings (i.e., a matrix whose columns contain the eigenvectors).



sdev	
the standard deviations of the principal components (i.e., the square roots of the eigenvalues of the covariance/correlation matrix, though the calculation is actually done with the singular values of the data matrix). 

```R
> eigen(d_cov)$values ^ 0.5
[1] 1.5811388 0.7071068
```



So you can see `Standard deviations` are the list of square root of eigenvalues



```R
d_cov = cov(d)
d_cov
eigen(d_cov)


     [,1] [,2]
[1,]  1.5  1.0
[2,]  1.0  1.5

eigen() decomposition
$values
[1] 2.5 0.5

$vectors
          [,1]       [,2]
[1,] 0.7071068 -0.7071068
[2,] 0.7071068  0.7071068
```



==i.e. if you want to know the rotated data==



```R
prcomp(d)$x 

# PC scores e.g. data point (-1,-2) is represented as
#  -2.1213203 * PC1    + 0.7071068 * PC2
# https://stats.stackexchange.com/a/282/272857

            PC1        PC2
[1,] -2.1213203  0.7071068
[2,] -0.7071068 -0.7071068
[3,]  0.0000000  0.0000000
[4,]  2.1213203  0.7071068
[5,]  0.7071068 -0.7071068


# rotated data on PC1
res_PC1 = d %*% ( prcomp(d)$rotation[,'PC1'])

t(t(d) - rowMeans(t(d))) %*% eigen(cov(d))$vectors
```



```R
prcomp(d)$x
# 结果横过去看就是 data point = f(PCs)
# 用新的坐标PC来表示数据
prcomp(d)$rotation
# 结果竖过来看就是 PC = f(features)
# 表示怎么用原来的坐标体系表达新的PC坐标体系
# 横过来看就是怎么用PC来返回来表达feature(x/y/z)坐标
```





==Note it is different from the Example b/c  we are using formula==

$cov(x,y) = \frac{1}{N-1} Z^TZ$

instead of

$cov(x,y) = \frac{1}{N} Z^TZ$

So in the example it is $1/5 * Z^TZ$ while in code it is $1/4 * Z^TZ$

> ?prcomp
>
> Unlike princomp, variances are computed with the usual divisor N - 1.



### Python

[Principal Component Analysis for Dimensionality Reduction in Python](https://machinelearningmastery.com/principal-components-analysis-for-dimensionality-reduction-in-python/?__s=j5spgttw6xiv8t5nozzl)

[how to do it in python mechanically](https://sebastianraschka.com/Articles/2015_pca_in_3_steps.html#a-summary-of-the-pca-approach)

https://github.com/heucoder/dimensionality_reduction_alo_codes/blob/master/codes/PCA/PCA.py#L13

## Resources



https://www.bilibili.com/video/av70839977?p=24

[STATQUEST Principal Component Analysis (PCA) Step-by-Step](https://youtu.be/FgakZw6K1QQ)

https://www.cnblogs.com/pinard/p/6239403.html

http://blog.codinglabs.org/articles/pca-tutorial.html

https://www.zhihu.com/question/30094611

[eigenvalues/eigenvectors](http://setosa.io/ev/eigenvectors-and-eigenvalues/)

[Understanding the Role of Eigenvectors and Eigenvalues in PCA Dimensionality Reduction.](https://medium.com/@dareyadewumi650/understanding-the-role-of-eigenvectors-and-eigenvalues-in-pca-dimensionality-reduction-10186dad0c5c)















