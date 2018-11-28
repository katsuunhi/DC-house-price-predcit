
### 竞赛时间安排
报名阶段：2018年8月28日-2018 年11月15日<br>
提交阶段：2018年11月01日 10:00--2018年11月27日10:00<br>
评审阶段：2018年11月27日-11月30日<br>
获奖公示：2018年12月1日-2018年12月3日 (比赛获奖公示，并接受异议、申诉和违规举报)<br>
>比赛链接： <http://www.dcjingsai.com/common/cmpt/住房月租金预测大数据赛（付费竞赛）_赛体与数据.html>

>感谢室友和学妹给了我这个经历

## 数据处理

>https://blog.csdn.net/qilixuening/article/details/75153131?tdsourcetag=s_pctim_aiomsg

1. jupyter notebook不支持中文，所以需要更改特证名
2. 分析缺省值，部分采用填0，部分采用取平均值，视特征属性而定
3. 分析各个特征的相关性，最后发现所有特征都用上效果更好，虽然有一部分相关性很低，原因不明
4. 离散变量编码

<br><br><br>

## 模型（xgboost,lightgbm,ExtraTreesRegressor,RandomForest,stacking,Ensemble learning）
1. __xgboost__（xgboost.ipynb）
    使用cv调参，最终参数定在了'max_depth':6,'min_child_weight':1,'learn_rate':0.1,'num_boost_round':19000，几个模型中排在 __第三位__ <br><br>


2. __lightgbm__(lightgbm.ipynb)
    运行速度非常快，调参非常方便，最终调参为num_leaves=78,learning_rate=0.05,n_estimators=5240,min_data_in_leaf=20，几个模型中排在 __第四位__ <br><br>


3. __ExtraTreesRegressor__(ExtraTreesRegressor-stacking.ipynb)
    调stacking的时候偶然发现的模型，跟其他模型比起来，未调参情况下分数更高，试用了一下，效果非常好，调参结果为n_estimators = 1000, max_depth=None, n_jobs = -1, min_samples_split =4,几个模型中排在 __第二位__ 
>https://www.cnblogs.com/massquantity/p/8640991.html<br>
>/Kaggle-HousePrices-master
<br><br>
4. __RandomForest__ 成绩很差<br><br>

5. __stacking__(ExtraTreesRegressor-stacking.ipynb)
    stacking通过将若干个模型结合，分成两层模型，第一层训练出数据，再放入第二层训练，采用了xgboost、lightgbm作为第一层，ExtraTreesRegressor作为第二层训练，历时一小时，成绩很差
>https://www.cnblogs.com/massquantity/p/8640991.html<br>
>/Kaggle-HousePrices-master
<br><br>
6. Ensemble learning
    调stacking过程中有个调不同模型组合权重的过程，采用了ExtraTreesRegressor和xgboost分别占用0.5的权重的比例，进行集成训练，成绩很好，最终得分 __最高__,后来又试了其他的比例，分数都会有所降低。
<br><br>

### 最终成绩 RMSE:1.81587，第一名1.72905，参赛总队伍447，A榜41名，B榜29名，最终按B榜成绩29名，二等奖
<br><br>

## 总结
第一次参加数据分析机器学习相关的比赛，之前也几乎没有接触过机器学习，从这次比赛，学习到了很多东西。<br><br>
    1. 数据预处理的方法和过程，通过python的很多库，可以很直观的分析数据分布和数据特征<br><br>
    2. 模型有很多，不能在一棵树上吊死，多试试其他模型，说不定就会有一个最好的，/Kaggle-HousePrices-master，stacking的过程中有一步是不同算法一起比较，这个方法我觉得很好，毕竟一下子就找出了extraTreesRegressor这个模型，其他模型的分数也基本和这个结果的排名相同<br><br>
    3. 每天5次提交量，如果我们可以好好利用的话，不至于只拿了个二等奖，程序猿，懒是原罪<br><br>
    4. 模型原理能够理解更好，不能理解也不影响使用，毕竟有数据就会出现结果，我们只需要让结果最好就行了<br><br>
    5. 训练的时候，先用split将数据集分割成train和test，根据test-mse得到最好模型，再使用model.train训练所有数据集，这样的得到的模型会比较好<br><br>
    6. cv又叫交叉验证，还有个什么grid，可以一次又一次反复的使用不同的参数进行组合，采用交叉验证的方式得到最好的参数组合，lightgbm可以用一下，其他模型很花时间，xgboost一直没能等出结果<br><br>