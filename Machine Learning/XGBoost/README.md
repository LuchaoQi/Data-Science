### XGBoost



[灵魂拷问：你看过Xgboost原文吗？](https://mp.weixin.qq.com/s?__biz=MzIyNjM2MzQyNg==&mid=2247488488&idx=1&sn=bb82f1b0730d0f40831f4e38d3f70053&chksm=e870c2a5df074bb3c9b5061ff9ab1e5a14651f8329eaafb704529e0fc3c778945ba5823e792e&mpshare=1&scene=1&srcid=&sharer_sharetime=1579560192788&sharer_shareid=54d7b6bf73b347d381a7bff3f78b99d1&key=47c0c8dda35b3d6e8d58be57f843ddf6e5700899235612f906d0cc9ec648ff847511dd7f05f238e5e0b4cdbc5c6358089353e5338ccd8c87997658c3308b2f067d443eef1b3f00e962163239872d4789&ascene=1&uin=NzA3NTE3MTMz&devicetype=Windows+10&version=62070158&lang=en&exportkey=AxPNgbmkMBnLxkfFDPp2hQQ%3D&pass_ticket=TiRsKQs%2FRQm%2B3Gov7686G0%2BCuJ1qOKs5gA97BDJP1Mvj%2Bfs0O0plDk%2FNsvzPWBRH)



[XGBoost Part1: XGBoost Trees for Regression](https://www.youtube.com/watch?v=OtD8wVaFm6E&feature=youtu.be)

>  Holy Moly, the Big Boss XGBoost has finally arrived. Thanks to STATQUEST!

### Notes

XGBoost Trees are more complicated than regression trees which just need to calculate the minimum SSR. In XGBoost, similar to Gradient Boost, we set a initial guess value and than do the following regression.

![](https://i.loli.net/2019/12/18/MKUuSFadLVnAsZm.png)

> We set the initial predicted value 0.5. In this case, we set $\lambda = 0$.

![](https://i.loli.net/2019/12/18/GSIM3pnABechrmK.png)

![](https://i.loli.net/2019/12/18/AgXn97fQWT23Cjt.png)

> Split the leaf until only one residual in the leaf (only $-10.5$ left in the leaf on the left). For simplicity sake, we just limit the tree depth to two levels.
>
> Then, we need to prune the XGBoost tree based on its Gain values.



![](https://i.loli.net/2019/12/18/pxct5z24hmIyqNX.png) 

> If the Gain of branch is larger than $\gamma$, we'll retain the branch. Note, we'll consider the order of branches from bottom up.

![](https://i.loli.net/2019/12/18/QYEKxWnd3MUwPGu.png)