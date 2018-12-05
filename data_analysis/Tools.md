### 数据分析工具

#### numpy
- 用途：数据标准化格式化
- 常用函数
    - `mean()`: 求均值（给定一个list）
    - `std()`: 求标准差（给定一个list）

#### pandas
- 用途：数据读取和格式化
- 依赖：numpy
- 特性：
    - 数据读取成矩阵，里面的一列就是一个numpy组
    ```
    data.csv
    ==========
    id name
    1   a
    2   b
    data = pandas.read_csv("data.csv")
    data.id.sum() # 抽取id一列，获取总数
    ```
    - 灵活的数据选取方式
        - 任意数字范围选择
        ```
        data.iloc[0:2,0:3] # 选取前三行，前四列的内容
        data.iloc[:,:] 选取所有数据
        data.iloc[:,1:] 选取所有行，第二列到所有列
        ```
        - 行+索引范围选择
        ```
        data.loc[:1, ["name", "test"]] # 选择0-1的行，以及列为name和test的列
        ```
        - 索引选取
        ```
        data[["score", "name"]] # 选择score和name两列（当然也可以只选一列）
        ```
        - 条件选取
        ```
        condition = data['score'] > 7 # 获取一个条件列
        # 条件列里面都是1/0, 支持条件列之间的逻辑运算
        data_conditioned = data[condition] # 根据条件列获取值
        ```
        
- 基本单元：Series: 一个单独的一列
    - 构造方式：`line = pandas.Series([1,2,3])`
- 组成结构： 多个Series组成的DataFrame
    - 构造方式：`df = pandas.DataFrame(["id", 1, 2], ['name', 'a', 'b'])`
    