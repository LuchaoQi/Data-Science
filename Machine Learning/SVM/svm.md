# Support Vector Machines



### Resources





[Loss Function(Part III): Support Vector Machine](https://towardsdatascience.com/optimization-loss-function-under-the-hood-part-iii-5dff33fa015d)

[hinge loss in SVM](https://stats.stackexchange.com/a/87160)





[Udacity](https://www.youtube.com/watch?v=5yzSv4jYMyI&list=PLgIPpm6tJZoShjm7r8Npia7CMsMlRWeuZ&index=1)



Another pretty good introduction

[Simple Tutorial on SVM and Parameter Tuning in Python and R](https://www.hackerearth.com/blog/developers/simple-tutorial-svm-parameter-tuning-python-r/)

![](https://blog-c7ff.kxcdn.com/blog/wp-content/uploads/2017/02/kernel.png)

[Support Vector Machines](https://medium.com/datadriveninvestor/support-vector-machines-ae0ff2375479)



[Support Vector Machine — Simply Explained](https://towardsdatascience.com/support-vector-machine-simply-explained-fee28eba5496)

[Support Vector Machines, Clearly Explained!!!](https://www.youtube.com/watch?v=efR1C6CvhmE&feature=youtu.be)

[Math behind the model (Chinese)](https://www.bilibili.com/video/av70839977/?p=28&spm_id_from=333.788.b_6d756c74695f70616765.28)



### Notes



Besides directly calculating the distance between point and plane (Euclidean distance)

Udacity provides another understanding of optimization problem.

For two vectors


$$
\begin{array}{l}{\omega^{\top} x_{1}+b=1} \\ {\omega^{\top} x_{2}+b=-1}\end{array}
$$


If you subtract them, the distance between planes (i.e. margin) can be presented as


$$
\frac{\omega^{T}\left(x_{1}-x_{2}\right)}{\|\omega\|}=\frac{2}{\|\omega\|}
$$

**Note $w$ is the direction vertical to the hyperplane so the left part means exactly the projected distance between two points!!!**

Maximizing the margin equals to minimizing the reciprocal along with monotone

$$
\begin{array}{l}{\max \frac{2}{\| w_{1}\|}} \\ {\min 1 / 2\|w\|^{2}}\end{array}
$$

So we can further use [Lagrange equation/ KKT conditions](https://github.com/LuchaoQi/Machine-Learning/blob/master/svm/handwritten%20derivation.pdf) to solve this problem.


$$
\begin{aligned} \min L(w, b, \alpha)=& \frac{1}{2}\|w\|^{2}+\sum_{i=1}^{m} \alpha_{i}\left(-y_{i}\left(w^{T} x_{i}+b\right)+1\right) \\ &=\frac{1}{2} w^{T} w-\sum_{i=1}^{m} \alpha_{i} y_{i} w^{T} x_{i}-b \sum_{i=1}^{m} \alpha_{i} y_{i}+\sum_{i=1}^{m} \alpha_{i} \\ &=\frac{1}{2} w^{T} \sum \alpha_{i} y_{i} x_{i}-\sum_{i=1}^{m} \alpha_{i} y_{i} w^{T} x_{i}+\sum_{i=1}^{m} \alpha_{i} \\ &=\sum_{i=1}^{m} \alpha_{i}-\frac{1}{2} \sum_{i=1}^{m} \alpha_{i} y_{i} w^{T} x_{i} \\ &=\sum_{i=1}^{m} \alpha_{i}-\frac{1}{2} \sum_{i, j=1}^{m} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} x_{j}\right) \end{aligned}
$$

$$
\begin{array}{l}{\max \sum_{i=1}^{m} \alpha_{i}-\frac{1}{2} \sum_{i, j=1}^{m} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} x_{j}\right)=\min \frac{1}{2} \sum_{i, j=1}^{m} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} x_{j}\right)-\sum_{i=1}^{m} \alpha_{i}} \\ {\text { s.t. } \sum_{i=1}^{m} \alpha_{i} y_{i}=0} \\ {\alpha_{i} \geq 0, i=1,2, \ldots, m}\end{array}
$$



Also, in soft margin we sometimes use `C` term, similar to L1/L2 regularization, we have the following equation

$$
\frac{1}{2}\|w\|^{2}+C \sum_{i} \max \left(0,1-y_{i}\left(w^{\top} x_{i}+b\right)\right)
$$

which helps us intuitively understand the `C` term.







### Example

[SVM 调参策略](https://blog.csdn.net/u014484783/article/details/78220646)

```python
# SVM Classifier using cross validation    
def svm_cross_validation(train_x, train_y):    
    from sklearn.grid_search import GridSearchCV    
    from sklearn.svm import SVC    
    model = SVC(kernel='rbf', probability=True)    
    param_grid = {'C': [1e-3, 1e-2, 1e-1, 1, 10, 100, 1000], 'gamma': [0.001, 0.0001]}    
    grid_search = GridSearchCV(model, param_grid, n_jobs = 8, verbose=1)    
    grid_search.fit(train_x, train_y)    
    best_parameters = grid_search.best_estimator_.get_params()    
    for para, val in list(best_parameters.items()):    
        print(para, val)    
    model = SVC(kernel='rbf', C=best_parameters['C'], gamma=best_parameters['gamma'], probability=True)    
    model.fit(train_x, train_y)    
    return model
```
