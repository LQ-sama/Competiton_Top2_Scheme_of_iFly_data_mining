# Top2_Scheme_of_iFly_competition
使用xgboost和ensemble策略对表格数据进行预测

以下是对各个代码文件的说明：
总述：本次比赛，我们主要在两部分下功夫：特征工程和模型构造。在特征工程用了重要性排序的筛选。在模型构造方面主要使用了xgboost模型。其中的一些细节（例如怎么调参），请见如下说明。

各个文件的说明：如果想要复现，请按照顺序运行以下代码文件：
choose_features.ipynb   ： 用来对51个特征进行重要性排序，并且筛选出重要性大于0.05的几个特征。
deep_fs.ipynb  ：  用来对刚刚找到的几个特征进行特征扩充。
tmp.ipynb   ： 把原本的数据集，和扩充出来的新特征，拼起来。
find_best_params.ipynb ： 寻找一会训练用的网络的最佳参数。
xg_play.ipynb  ： 使用xgboost训练并且预测。


其中各个代码中的一些小细节在此说明（在每个代码中也有相应位置的注释）：
choose_features.ipynb ：为什么选择0.05为阈值：因为考虑到共有51个特征，理想的重要性平均值大约是0.02，所以稍微高一点，选择了0.05为阈值

deep_fs.ipynb  ：为什么选择加、减、乘来产生新特征，却没有除：因为原数据里面有0，如果选择除法，会出现inf的异常值，不利于训练

tmp.ipynb   ：没啥，就是把原有特征和新特征拼起来。

find_best_params.ipynb ：按照 max_depth, min_child_weight, n_estimators, gamma, reg_alpha的顺序挨个进行参数寻找。其中scale_pos_weight 的值是拿训练集中，1的个数除以0的个数得到的
                                          在xg_play.ipynb文件中，各个超参数都是已经调好的了，所以这个寻找参数的文件只是用来展示，我们是怎么找到的各个超参数。

xg_play.ipynb  ： 实验发现，nums超参数和n_estimators_list超参数（也就是下面的512，和list(range(100,175,2))，在我们实验的范围内，取得越大，f1分数会更高。对于nums，我们尝试了从1到512的一些取值：例如3、15、56、128、256、512,对于n_estimators_list： 我们尝试了从97到300的一些取值，但是由于训练时间关系，我们并没有尝试更大的数值。
