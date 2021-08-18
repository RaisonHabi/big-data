## 数据中心整体架构
数据仓库的整理架构，各个系统的元数据通过ETL同步到操作性数据仓库ODS中，对ODS数据进行面向主题域建模形成DW（数据仓库），DM是针对某一个业务领域建立模型，具体用户（决策层）查看DM生成的报表。

图表见链接

&nbsp;
## 数据引入层（ODS）
ODS（Operational Data Store）层存放您从业务系统获取的最原始的数据，是其他上层数据的源数据。业务数据系统中的数据通常为非常细节的数据，经过长时间累积，且访问频率很高，是面向应用的数据。

### 数据引入层表设计
本教程中，在ODS层主要包括的数据有：交易系统订单详情、用户信息详情、商品详情等。这些数据未经处理，是最原始的数据。逻辑上，这些数据都是以二维表的形式存储。虽然严格的说ODS层不属于数仓建模的范畴，但是合理的规划ODS层并做好数据同步也非常重要。本教程中，使用了6张ODS表：
```
记录用于拍卖的商品信息：s_auction。
记录用于正常售卖的商品信息：s_sale。
记录用户详细信息：s_users_extra。
记录新增的商品成交订单信息：s_biz_order_delta。
记录新增的物流订单信息：s_logistics_order_delta。
记录新增的支付订单信息：s_pay_order_delta。
```

&nbsp;
## References
[数据仓库ODS、DW和DM概念区分](https://www.jianshu.com/p/72e395d8cb33)   
[数据引入层（ODS）](https://help.aliyun.com/document_detail/117440.html)
