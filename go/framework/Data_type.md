## Gin的基本数据结构

### `Engine`
引擎结构（核心）
- 数据成员
    - `RouterGroup`：保存结构体`RouterGroup`
    - `trees MethodTrees`: 保存`methodTree`变量切片，初始化时容量为9
    - `delims`: 用于渲染模板时变量的分隔符，默认为`{{}}`
    - `pool`：同步池，
    - 其他

- 函数成员
    - `New()`
    
    - `Default()`
    
    

### `RouterGroup`
- 数据成员
    - `Handlers HandlersChain`: 处理器`HandlerFunc`的slice，保存当前的一堆处理器
    - `basePath string`：根位置，默认为"/"
    - `engine *Engine`：默认为调用的engine对象自身
    - `root bool`：默认true