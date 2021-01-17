# shap explain
按照官网的例子，来个实例解释下，整体见下图
![2021-01-15-15-53-47](http://img.no1token.com/2021-01-15-15-53-47.png)

具体细节解释见官网

现以一部分实例代码：

``` python
import xgboost
import shap
# X,y = shap.datasets.boston()
model = xgboost.train({"learning_rate": 0.01}, xgboost.DMatrix(X, label=y), 100)
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X)
shap.summary_plot(shap_values, X)
```

现在针对主要分布情况解释：
- 混杂 ： 不是一个特别有解释的特征
![2021-01-17-09-01-51](http://img.no1token.com/2021-01-17-09-01-51.png)
- 区间颜色比较分明的： 是一个有区分度的特征
![2021-01-17-09-02-27](http://img.no1token.com/2021-01-17-09-02-27.png)
- 左右颜色比较分明并且x轴单边/双边长度长的： 是一个有高区分度的特征
![2021-01-17-09-01-19](http://img.no1token.com/2021-01-17-09-01-19.png)

