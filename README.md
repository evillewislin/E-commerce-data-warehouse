# E-commerce-data-warehouse
电商数据仓库实战项目
1、项目介绍
功能
    采集项目：以数据的采集和传输为主。
    数据仓库项目：以数据的计算为主，也能存储数据。
 技术
    采集项目：flume、kafka、datax、maxwell。
    数据仓库项目：Mysql、HDFS、Spark、Fink、MR、Hive。
 2、数据库和数据仓库的区别
名称
    数据库：database（基础，核心的数据）
    数据仓库：Data Warehouse（数据的货栈）
数据来源
    数据库：企业中基础核心的夜晚数据
    数据仓库：数据库中的数据
数据存储
    数据库：核心是查找业务数据。
            行式存储
            索引
            不能存储海量数据
    数据仓库：核心是统计分析数据
            列式存储
            存储海量数据
3、数据流转过程
    用户→业务服务器→数据存储（业务数据和行为数据）→数据统计分析（数据仓库）→MySQL→数据可视化
4、统计分析的基本步骤
    数据仓库核心功能：统计分析
        Spark On Hive：Spark解析SQL
        Hive On Spark：Hive解析SQL        
    数据源→加工数据→统计数据→分析数据
5、架构
    数据存储→数据仓库Hive（数据源、加工数据、统计数据、分析数据）→数据可视化   
数据源   
    如果将数据库直接作为数据仓库的数据源
        1、存储方式不同，一个行式，应该列式。
        2、不是海量数据，要求海量数据。
        3、对数据库的性能造成影响。
    数据仓库有自己的一个数据源，代替和补充数据库。
    数据存储是和数据库同步的。
数据采集
    数据仓库的数据源数据需要从数据库周期性同步过来。
    使用DataX和Maxw、flume采集到HDFS解Hive的耦合
数据同步的方式：
    全量数据同步：表的全部数据。
    增量数据同步：表的新增及变化的数据。
    采集数据的目的，为了后期的统计分析做准备
        需求1：当前电商网站所以得注册用户的数量
            Select count(*) from t_user
        需求2：当前电商网站最近1周的新增注册用户的数量
            Select count(*) from t_user where regdate >= 10-05 and regdate <= 10-12
            增量生成的一个表 t_user_7
            Select count(*) from t_user_7
数据同步格式：
    1、用户行为日志数据
        格式：JSON 
        1.1 页面浏览日志
            common：环境信息
            action：动作信息
            displays：曝光信息
            page：页面信息
            err：错误信息
            ts：进入当前页面的时间戳（毫秒）
        1.2 APP启动日志
            common：环境信息
            start：启动信息
            err：错误信息
            ts：启动APP时间戳（毫秒）
日志数据采集
    Log