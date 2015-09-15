# 类文件的结构
##类文件中的字节序
Big-Endian，大端序
##方法特征签名
方法表集合中存储了方法特征签名
###Java代码方法特征签名包
1. 方法名称
2. 参数顺序
3. 参数类型

所以重载的要求是：
- 拥有与原方法不同的**特征签名**

##Class文件格式
| 类型 |名称|物理意义|数量|
|--|--|--|--|
|u4|magic|魔数|1|
|u2|minor_version|版本号|1|
|u2|major_version|版本号|1|
|u2|constant_pool_count|常量池入口|1|
|cp_info|constant_pool|常量池入口|1|
|u2|access_flags|访问标志|1|
|u2|this_class|类索引|1|
|u2|super_class|类索引|1|
|u2|interface_count|接口索引集合|1|
|u2|interface|接口索引集合|interface_count|
|u2|fields_count|字段表|1|
|u2|fields|字段表|fields_count|
|field_info|fields|字段表|fields_count|
|u2|methods_count|方法表集合|methods_count|
|method_info|methods|方法表集合|methods_count|
|u2|attribute_count|属性表|1|
|attribute_info|attributes|属性表|attributes_count|