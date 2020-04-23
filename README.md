# pandaslearning
2020年4月20日至2020年4月30日，初步系统学习pandas

2020年4月20日 pandas基础笔记

一、文件读取与写入
pd.read_csv('csv文件名')           .to_csv('csv文件名')
pd.read_table('txt文件名')
pd.read_excel('excel文件名')       .to_excel('excel文件名',sheet_name= )

二、基本数据结构
1. Series
（a）创建一个Series        最常用的属性为值（values），索引（index），名字（name），类型（dtype）
pd.Series(valus,index,name,dtype=)
（b）访问Series属性        .属性即可
（c）取出某一个元素
（d）调用方法

2. DataFrame
（a）创建一个DataFrame               pd.DataFrame({'col1':list('abcde'),'col2':range(5,10),
                                                 'col3':[1.3,2.5,3.6,4.6,5.8]},index=list('一二三四五'))
（b）从DataFrame取出一列为Series     df['列名']
（c）修改行或列名                   .rename(index={'原':'新'},columns={'原':'新'})
（d）调用属性和方法                 .属性即可：index,values,columns,shape,mean
（e）索引对齐特性                    
df1 = pd.DataFrame({'A':[1,2,3]},index=[1,2,3])
df2 = pd.DataFrame({'A':[1,2,3]},index=[3,1,2])
df1-df2 #由于索引对齐，因此结果不是0
（f）列的删除与添加                 
.drop(index='五',columns='col1') #设置inplace=True后会直接在原DataFrame中改动
del df['列名']
.pop('列名')  返回弹出的列
可以直接增加新的列，也可以使用assign方法，但assign方法不会对原DataFrame做修改
.assign(C=pd.Series(list('def')))
（g）根据类型选择列           .select_dtypes(include=['number']).head()       
（h）将Series转换为DataFrame    .to_frame()
使用T符号可以转置

三、常用基本函数
1. head和tail     头尾几行数据
2. unique和nunique    nunique显示有多少个唯一值，unique显示所有的唯一值
3. count和value_counts    count返回非缺失值元素个数，value_counts返回每个元素有多少个
4. describe和info         info函数返回有哪些列、有多少非缺失值、每列的类型，describe默认统计数值型数据的各个统计量，对于非数值型也可以用describe函数
5. idxmax和nlargest      idxmax函数返回最大值，在某些情况下特别适用，idxmin功能类似，nlargest函数返回前几个大的元素值，nsmallest功能类似
6. clip和replace          clip和replace是两类替换函数，clip是对超过或者低于某些值的数进行截断，replace是对某些值进行替换
7. apply函数             对于Series，它可以迭代每一列的值操作：.apply(lambda x:str(x)+'!').head() #可以使用lambda表达式，也可以使用函数
                         对于DataFrame，它可以迭代每一个列操作：.apply(lambda x:x.apply(lambda x:str(x)+'!')).head() 
                        
四、排序
1. 索引排序              .set_index('Math').head()
2. 值排序                .sort_values(by='Class').head()
多个值排序，即先对第一层排，在第一层相同的情况下对第二层排序


五、问题与练习
1. 问题
【问题一】 Series和DataFrame有哪些常见属性和方法？
series:值（values），索引（index），名字（name），类型（dtype）
dataframe:index,values,columns,shape,mean

【问题二】 value_counts会统计缺失值吗？
不会，values_counts统计每个元素的个数

【问题三】 与idxmax和nlargest功能相反的是哪两组函数？
idxmin         nsmallest

【问题四】 在常用函数一节中，由于一些函数的功能比较简单，因此没有列入，现在将它们列在下面，请分别说明它们的用途并尝试使用。
sum/mean/median/mad/min/max/abs/std/var/quantile/cummax/cumsum/cumprod
求和/求平均/中位数/根据平均值计算绝对离差/最小值/最大值/绝对值/标准差/方差/四分位数/样本值累计最大值/样本值累计和/样本值累计积

【问题五】 df.mean(axis=1)是什么意思？它与df.mean()的结果一样吗？第一问提到的函数也有axis参数吗？怎么使用？
df.mean(axis=1)：行平均值；df.mean(axis=0)：列平均值

2. 练习
【练习一】 现有一份关于美剧《权力的游戏》剧本的数据集，请解决以下问题：
（a）在所有的数据中，一共出现了多少人物？
df['name'].nunique()
（b）以单元格计数（即简单把一个单元格视作一句），谁说了最多的话？
df['name'].values_counts().index[0]
（c）以单词计数，谁说了最多的单词？
?

【练习二】现有一份关于科比的投篮数据集，请解决如下问题：
（a）哪种action_type和combined_shot_type的组合是最多的？
?
（b）在所有被记录的game_id中，遭遇到最多的opponent是一个支？
?


2020年4月20日 索引笔记

一、单级索引
1. loc方法、iloc方法、[]操作符
最常用的索引方法可能就是这三类，其中iloc表示位置索引，loc表示标签索引，[]也具有很大的便利性，各有特点
（a）loc方法
① 单行索引： .loc[行号]
② 多行索引： .loc[行号，行号]   或者.loc[行号：行号]
（注意：所有在loc中使用的切片全部包含右端点！这是因为如果作为Pandas的使用者，那么肯定不太关心最后一个标签再往后一位是什么，但是如果是左闭右开，那么就很麻烦，先要知道再后面一列的名字是什么，非常不方便，因此Pandas中将loc设计为左右全闭）
③ 单列索引： .loc[：，'列名']
④ 多列索引： .loc[：，['列名','列名']]
⑤ 联合索引： .loc[行号：行号：间隔，'列名']
⑥ 函数式索引： .loc[lambda x:x['Gender']=='M']
⑦ 布尔索引： .loc[[True if i[-1]=='4' or i[-1]=='7' else False for i in df['Address'].values]]

（b）iloc方法（注意与loc不同，切片右端点不包含）

（c） []操作符
（c.1）Series的[]操作
① 单元素索引：
② 多行索引：
③ 函数式索引：
④ 布尔索引：

（c.2）DataFrame的[]操作
① 单行索引：
② 多行索引：
③ 单列索引：
④ 多列索引：
⑤函数式索引：
⑥ 布尔索引：

2. 布尔索引¶
（a）布尔符号：'&','|','~'：分别代表和and，或or，取反not
（b） isin方法

3. 快速标量索引
当只需要取一个元素时，at和iat方法能够提供更快的实现：

4. 区间索引
此处介绍并不是说只能在单级索引中使用区间索引，只是作为一种特殊类型的索引方式，在此处先行介绍
（a）利用interval_range方法
（b）利用cut将数值列转为区间为元素的分类变量，例如统计数学成绩的区间情况：
（c）区间索引的选取



二、多级索引
1. 创建多级索引
（a）通过from_tuple或from_arrays
① 直接创建元组
② 利用zip创建元组
③ 通过Array创建

（b）通过from_product

2. 多层索引切片
（a）一般切片
（b）第一类特殊情况：由元组构成列表
（c）第二类特殊情况：由列表构成元组


3. 多层索引中的slice对象
（a）loc[idx[*,*]]型
（b）loc[idx[*,*],idx[*,*]]型


4. 索引层的交换¶
（a）swaplevel方法（两层交换）
（b）reorder_levels方法（多层交换）


三、索引设定
1. index_col参数
2. reindex和reindex_like
3. set_index和reset_index
4. rename_axis和rename

四、常用索引型函数
1. where函数
2. mask函数
3. query函数


五、重复元素处理
1. duplicated方法
2. drop_duplicates方法


六、抽样函数
这里的抽样函数指的就是sample函数
（a）n为样本量
（b）frac为抽样比
（c）replace为是否放回
（d）axis为抽样维度，默认为0，即抽行
（e）weights为样本权重，自动归一化


七、问题与练习
1. 问题
【问题一】 如何更改列或行的顺序？如何交换奇偶行（列）的顺序？
【问题二】 如果要选出DataFrame的某个子集，请给出尽可能多的方法实现。
【问题三】 query函数比其他索引方法的速度更慢吗？在什么场合使用什么索引最高效？
【问题四】 单级索引能使用Slice对象吗？能的话怎么使用，请给出一个例子。
【问题五】 如何快速找出某一列的缺失值所在索引？
【问题六】 索引设定中的所有方法分别适用于哪些场合？怎么直接把某个DataFrame的索引换成任意给定同长度的索引？
【问题七】 对于多层索引，怎么对内层进行条件筛选？
【问题八】 swaplevel中的axis参数为1时，代表什么意思？i和j只能是数值型吗？

2. 练习
【练习一】 现有一份关于UFO的数据集，请解决下列问题：
（a）在所有被观测时间超过60s的时间中，哪个形状最多？
（b）对经纬度进行划分：-180°至180°以30°为一个经度划分，-90°至90°以18°为一个维度划分，请问哪个区域中报告的UFO事件数量最多？¶
【练习二】 现有一份关于口袋妖怪的数据集，请解决下列问题：
（a）双属性的Pokemon占总体比例的多少？
（b）在所有种族值（Total）不小于580的Pokemon中，非神兽（Legendary=False）的比例为多少？
（c）在第一属性为格斗系（Fighting）的Pokemon中，物攻排名前三高的是哪些？
（d）请问六项种族指标（HP、物攻、特攻、物防、特防、速度）极差的均值最大的是哪个属性（只考虑第一属性，且均值是对属性而言）？¶
（e）哪个属性（只考虑第一属性）神兽占总Pokemon的比例最高？该属性神兽的种族值也是最高的吗？
