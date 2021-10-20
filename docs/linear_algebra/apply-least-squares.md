在[前一节][1]中，我们得到对于系数矩阵B，有：

![](apply-least-squares/formula-1.jpg ':size=200')



假想我们手头上有一个数据集，里面有 3 条数据记录。每条数据记录有 2 维特征，也就是 2 个自变量，和 1 个因变量。

![](apply-least-squares/data.webp ':size=500')

如果我们假设这些自变量和因变量都是线性的关系，那么我们就可以使用如下这种线性方程，来表示数据集中的样本：

![](apply-least-squares/formula-2.jpg ':size=200')



让我们从最基本的几个矩阵开始：

![](apply-least-squares/formula-3.webp ':size=500')



矩阵 <big>(X’X)</big><sup>−1</sup> 的解法怎么做呢？看下面。

**如何求逆矩阵**

在[高斯消元法][2]里提到消元和回代的过程，就是把系数矩阵变为单位矩阵的过程，我们可以利用这点，来求解 <big>X</big><sup>−1</sup>。我们把原始的系数矩阵 X 列在左边，然后把单位矩阵列在右边，像 <big>[ X | I ]</big> 这种形式，其中 I 表示单位矩阵。

然后我们对左侧的矩阵进行高斯**消元**和**回代**，把左边矩阵 X 变为单位矩阵。同时，我们也把这个相应的矩阵操作运用在右侧。这样当左侧变为单位矩阵之后，那么右侧的矩阵就是原始矩阵 X 的逆矩阵 <big>X</big><sup>−1</sup>，具体证明如下：

![](apply-least-squares/formula-4.jpg ':size=150')



好了，给定下面的 <big>X’X</big> 矩阵之后，我们使用上述方法来求 <big>(X’X)</big><sup>−1</sup> 。我把具体的推导过程列在了这里（注意，消元是变成上三角矩阵，回代则是变成单元矩阵，下边的步骤里有跳过，请自行复习[高斯消元][2]）：

![](apply-least-squares/formula-5.jpg ':size=500')

求出  <big>(X’X)</big><sup>−1</sup> 之后可以计算出系数矩阵B：

![](apply-least-squares/formula-6.jpg ':size=500')



也就是说 <big>b</big><sub>1</sub><big>=1</big>, <big>b</big><sub>2</sub><big>=1.5</big>，在这个例子中得到的是精确解，如果改一下方程组会得到一个近似解：

![](apply-least-squares/formula-7.jpg ':size=500')

然后看看偏差值会有多少？

首先我们通过系数矩阵 B 和自变量矩阵 X 计算出来预测值。

![](apply-least-squares/formula-8.jpg ':size=400')



然后是样本数据中的观测值。这里我们假设这些值是真实值。

![](apply-least-squares/formula-9.jpg ':size=200')

根据误差 ε 的定义，我们可以得到：

![](apply-least-squares/formula-10.webp ':size=500')

## 总结

简短地总结一下，线性回归模型根据大量的训练样本，推算出系数矩阵 B，然后根据新数据的自变量 X 向量或者矩阵，计算出因变量的值，作为新数据的预测。

**如何判断一个数据集是不是能用线性模型表示呢？**

在线性回归中，我们可以使用决定系数 R2。这个统计指标使用了回归平方和与总平方和之比，是反映模型拟合度的重要指标。它的取值在 0 到 1 之间，越接近于 1 表示拟合的程度越好、数据分布越接近线性关系。

随着自变量个数的增加，R2 将不断增大，因此我们还需要考虑方程所包含的自变量个数对 R2 的影响，这个时候可使用校正的决定系数 Rc2。

所以，在使用各种科学计算库进行线性回归时，你需要关注 R2 或者 Rc2，来看看是不是一个好的线性拟合。sklearn 的 regression.score 函数，其实就是返回了线性回归的 R2。

```python
import pandas as pd
from sklearn.linear_model import LinearRegression

df = pd.read_csv("linear-regression.csv")
df_features = df.drop(['y'], axis=1)     #Dataframe中除了最后一列，其余列都是特征，或者说自变量
df_targets = df['y']                     #Dataframe最后一列是目标变量，或者说因变量

print(df_features, df_targets)
#使用特征和目标数据，拟合线性回归模型
regression = LinearRegression().fit(df_features, df_targets)
#拟合程度的好坏, 决定系数R2
print(regression.score(df_features, df_targets))
#截距
print(regression.intercept_)
#各个特征所对应的系数
print(regression.coef_)
```

## python代码

见 notebooks/linear-regression.ipynb

[1]: linear_algebra/least-squares

[2]: linear_algebra/gaussian-elimination
