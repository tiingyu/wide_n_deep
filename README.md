
REFERENCE: https://github.com/tensorflow/tensorflow/blob/r1.3/tensorflow/examples/learn/wide_n_deep_tutorial.py
           https://github.com/tensorflow/models/tree/master/official/wide_deep  
           https://blog.csdn.net/cjopengler/article/details/78161748
           https://blog.csdn.net/u014021893/article/details/80423112
DATASET: census
PYTHON: py3
CONTENT:
1、download: train_data, test_data

2、(1) 连续变量,转化为浮点数：tf.feature_column.numeric_column()
例：age、education_num、capital_gain、capital_loss、hours_per_week

(2) 分布不均匀的连续变量,分块处理离散化, 丰富特征，构造非线性特征：tf.feature_column.bucketized_column(),得到的是sparsercolumn
例：age 得到age_bucket

(3) 取值较少的类别特征：tf.feature_column.categorical_column_with_vocabulary_list()得到的是sparsercolumn
例：gender、education、marital_status、relationship、workclass

(4) 取值较多或者不清楚有多少取值的类别特征：tf.feature_column.categorical_column_with_hash_bucket()进行哈希分桶得到的是sparsercolumn
例：occupation、native_country   

3、(1) modeltype='wide',只用LR，使用base_columns：线性模型可以直接使用sparsercolumn,因此上面得到的变量可以直接使用
gender, education, marital_status, relationship, workclass, occupation, native_country, age_buckets

(2) modeltype='deep',只用DNN,使用deep_columns：深度模型需要将sparsercolumn转换成densecolumn
对workclass、education、gender、relationship使用tf.feature_column.indicator_column()进行one-hot|multi-hot编码，转换为densecolumn
对native_country、occupation( 使用categoryical_column产生的sparsor_column)使用tf.feature_column.embedding_column()转化为densecolumn
使用连续变量：age, education_num, capital_gain, capital_loss, hours_per_week

(3) model_type='wide_deep',使用wide&deep，其中：
LR：组合特征 ["education", "occupation"], [age_buckets, "education", "occupation"], ["native_country", "occupation"]
(使用 tf.feature_column.crossed_column() 组合系数特征，这仅仅适用于sparser特征.产生的依然是sparsor特征)?
DNN：使用deep时的变量

3、模型
reference: https://blog.csdn.net/sxf1061926959/article/details/78440220
           https://github.com/tensorflow/tensorflow/blob/r1.12/tensorflow/contrib/learn/python/learn/estimators/dnn_linear_combined.py
           
          
           
           






