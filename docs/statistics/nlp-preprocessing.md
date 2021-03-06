## 词包模型

Bag of words 模型。计算机处理自然语言的基本单位是单词和词组。词包模型是一种把文本分割成单词和词组的**基于自然语言的预处理**模型。

* 忽略文本中的词语出现的顺序以及相应的语法
* 将整篇文章仅仅看做是一个大量单词的组合
* 每个词的出现都是独立的（看到这里应该想到 [朴素贝叶斯](statistics/naive-bayes)）

## 1. 分词

* 对于英语等拉丁语系的语言来说，使用空格对句子进行分割。
* 对于中文、日文、韩文，使用一些算法来**估计**词语之间的划分。

分词模型：

### 基于字符串匹配

扫描字符串，如果发现字符串的子串和词相同，就算匹配成功。

匹配规则通常是：正向最大匹配、逆向最大匹配、长词优先。

优点：只需使用基于字典的匹配，因此计算复杂度低。

缺点：处理歧义词效果不佳。

### 基于统计和机器学习

基于人工标注的词性和统计特征，对中文进行建模。训练阶段，根据标注好的语料对模型参数进行估计。 在分词阶段再通过模型计算各种分词出现的概率，将概率最大的分词作为最终结果。

常见的序列标注模型：

* 隐马尔科夫模型（HMM，Hidden Markov Model）
* 条件随机场（CRF，Conditional Random Field）

### 其他模型

TODO

## 2. 取词干和归一化

英文的同一单词具有单复数、各种时态，因此它需要考虑**取词干（stemming）**。比如：

* 将am，is，are，was，were全部转换为be
* 将car，cars，car’s，cars’全部转换为car

大小写转化和多种拼写（例如 color 和 colour）这样的统一化，称为**归一化**。

## 3. 停用词

不影响（或基本不影响）相关性的词。有的时候干脆可以指定一个叫**停用词**（stop word）的字典，直接将这些词过滤。例如英文中的 a、an、the、that、is、good、bad 等。中文“的、个、你、我、他、好、坏”等。

这样，可以在基本不损失语义的情况下，减少数据文件的大小，提高计算机处理的效率。

注意停用词的使用场景，某些场景下，某些词反而是关键，而不是停用词。

## 4. 同义词和扩展词

同义词的词典，比如：

```
番茄，西红柿
菠萝，凤梨
洋山芋，土豆
泡面，方便面，速食面，快餐面
山芋，红薯
鼠标，滑鼠
……
```

当看到文本中出现“番茄”关键词的时候，计算机系统就会将其等同于“西红柿”这个词

有的时候我们还需要**扩展词**。如果简单地将 Dove 分别和多芬、德芙简单地等价，那么多芬和德芙这两个完全不同的品牌也变成了同义词，这样做明显是有问题的。那么我们可以采用扩展关系，当系统看到文本中的“多芬”时将其等同于“Dove”，看到“德芙”时将其等同于“Dove”。但是看到“Dove”的时候并不将其等同于“多芬”或“德芙”。