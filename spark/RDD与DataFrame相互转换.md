## 示例：
**RDD转换成DataFrame**  
   employee_temp = spark.createDataFrame(employee)  
    **显示DataFrame数据**  
    employee_temp.show()   
    **创建视图**  
    employee_temp.createOrReplaceTempView("employee")  
    **过滤数据**  
    employee_result = spark.sql("SELECT name,salary FROM employee WHERE salary >= 14000 AND salary <= 20000")  
    **DataFrame转换成RDD**  
    result = employee_result.rdd.map(lambda p: "name: " + p.name + "  salary: " + str(p.salary)).collect()  
    **打印RDD数据**  
    for n in result:  
        print(n)  
        
[spark: RDD与DataFrame之间的相互转换](https://blog.csdn.net/zhurui_idea/article/details/73090951)
