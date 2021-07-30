## 方差公式
m为x1,x2...xn数列的期望值（平均数）

s^2 = [(x1-m)^2 + (x2-m)^2 + ... (xn-m)^2]/n

s即为标准差,s^2为方差。

&nbsp;
## hive函数
### 1.标准差
#### stddev_pop = stddev
#### stddev_samp  
s^2 = [(x1-m)^2 + (x2-m)^2 + ... (xn-m)^2]/**(n-1)**
### 2.方差
#### var_samp=stddev_samp^2
#### var_pop=stddev_pop^2

### 3.总结
当我们需要真实的标准差 方差的时候 最好是使用： stddev stddev_pop  var_pop

而只是需要得到少量数据 标准差 方差 的近似值 可以选用： stddev_samp var_samp

&nbsp;
## References
[hive函数 -- stddev , stddev_pop , stddev_samp , var_pop , var_samp](https://blog.csdn.net/lxpbs8851/article/details/39317611)
