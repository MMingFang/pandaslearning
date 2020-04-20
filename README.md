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
