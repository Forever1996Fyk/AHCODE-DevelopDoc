# 编码规范

以下编码规范全部根据阿里巴巴规范手册进行编写。

## 命名风格

### 基本命名风格

1. 代码中所有的命名必须全英文或者全拼音, 不允许使用拼音与英文混合, 更不能用中文
2. 类名使用`UpperCamelCase`风格, 以下情形除外: `DO / BO / DTO / VO / AO/ PO / UID`
3. 方法名, 参数名, 成员变量, 局部变量都统一使用`lowerCamelCase`风格, 必须遵守驼峰式
4. 常量名全部大写, 单词间用下划线隔开
5. 包名统一使用小写, 且用单数形式, 但是类名可以用复数形式

### 代码风格

1. 类方法命名(强制)

    - 获取单个对象方法用`get`前缀
    - 获取多个对象方法用`list`前缀
    - 获取统计值方法用`count`前缀
    - 插入方法用`insert`前缀
    - 删除方法用`delete`前缀
    - 修改方法用`update`前缀

2. 领域模型命令(强制)

    - 数据对象: xxxDO, xxx为数据表名
    - 数据传输对象: xxxDTO, xxx为业务领域相关名称
    - 展示对象: xxxVO, xxx一般为网页名称
    - `POJO`是`DO/DTO/BO/VO`的统称

3. OOP规范(强制)

    - 不能使用过时的类或方法
    - Object的equals方法容易抛空指针异常, 应使用常量或确定有值的对象来调用equals。`"test".equlas(object)`
    - 包装类对象之间值得比较全部使用equals方法(对于 Integer var = ? 在-128 至 127 范围内的赋值，Integer 对象是在 IntegerCache.cache 产生，会复用已有对象，这个区间内的 Integer 值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用 equals 方法进行判断。)
    - 定义数据对象DO类时, 属性类型要与数据库字段类型相匹配
    - 所有POJO类属性必须使用包装类型数据
    - 构造方法禁止加入任何业务逻辑, 如果有初始化逻辑, 请放在init方法中
    - POJO类必须重写`toString`方法

4. 集合处理规范(强制)

    - 所有集合判断空, 必须使用`CollectionUtils.isEmpty(list)`方法
    - 使用集合转数组的方法, 必须使用集合的`list.toArray(T[] array)`, 传入的类型完全一致, 长度为0的空数组
    - 不要在`foreach`循环里进行元素的`remove/add`操作。`remove`元素请使用`Iterator`方法, 如果并发操作, 需要对`Iterator`对象加锁

5. 注释(强制)

    - 类, 类属性, 类方法的注释都鄙视使用`/** 内容 */`格式, 不能使用`// xxx`方式
    - 所有抽象方法必须要用注释, 除了返回值, 参数, 异常说明, 还必须指出该方法做的什么事, 实现什么功能

6. 异常处理(强制)

    - 不要再`finally`块中使用`return`
    - 捕获异常是为了处理它, 必须将异常转化为用户可以理解的内容
    - 有try块放到了事务代码中, catch异常后, 如果需要回滚事务, 必须要手动回滚事务

7. 日志规范(强制)

    - 系统中不能直接使用`Log4j`, `Logback`中的API, 应该依赖日志框架`Slf4j`的API, 使用门面模式的日志框架, 有利于维护各个类的日中处理方式统一
    - 日志文件至少保留15天
    - 日志输出, 字符串变量的拼接使用占位符操作
    - 对于`trace/debug/info`级别日志输出, 必须进行日志级别的开关判断
        ```java
            if (logger.isDebugEnabled) {
                logger.debug("Current ID is: {} and name is: {}", id, getName());
            }
        ```

## 数据库规范

### 建表规约

1. 表达是否概念的字段, 必须使用`is_xxx`方式命名, 数据类型是`unsigned tinyint`(1表示是, 0表示否)
2. 表名不使用复数名词
3. 禁用保留字
4. 小数类型为`decimal`, 禁止使用`float`和`double`
5. varchar是可变长字符串, 不预先分配存储空间, 长度不要超过5000, 如果存储长度大于5000, 定义字符类型为text, 独立出来一张表, 用主键来对应, 避免影响其他字段索引效率。
6. 表必备字段: `id, create_time, update_time` 。`create_time, update_time`的类型均为`datetime`类型

### ORM映射

1. 表查询中, 一律不能使用 * 作为查询字段的列表
2. POJO类的布尔属性不能加is, 而数据库字段必须加is
3. sql.xml配置参数使用: #{}, #param#, 不是使用${}
4.  不允许直接使用`HashMap`与`HashTable`座位查询结果集的输出



