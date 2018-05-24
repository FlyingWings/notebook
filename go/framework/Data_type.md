## Gin的基本数据结构

### `Engine`
引擎结构（核心）
- 数据成员
    - `RouterGroup`：保存结构体`RouterGroup`，匿名包含（继承）
    - `trees MethodTrees`: 保存`methodTree`变量切片，初始化时容量为9
    - `delims`: 用于渲染模板时变量的分隔符，默认为`{{}}`
    - `pool`：同步池，@todo
    - 其他
    

### `RouterGroup`
- 用途：内部配置路由用，实际上是一个路由组
- 数据成员
    - `Handlers HandlersChain`: 处理器`HandlerFunc`的slice，保存当前的一堆处理器
    - `basePath string`：根位置，默认为"/"
    - `engine *Engine`：默认为调用的engine对象自身指针
    - `root bool`：默认true
    
    
- 方法成员
    - handle: 用于定义请求方法，请求路径和请求函数(只是定义，不是调用！)
        - 定义：`func (group *RouterGroup) handle(httpMethod, relativePath string, handlers HandlersChain) IRoutes`
    
    
### `Context`
- 用途：Gin核心上下文，用于传递变量，管理流（输入输出）等
- 数据成员：
    - `handlers`: 同上，`HandlersChain`
    - `engine`: 保存当前存储的`*Engine`
    - `Request`: 请求对象
    - `writermem`: 响应输出器