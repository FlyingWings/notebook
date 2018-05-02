## Nginx基本数据结构

#### ngx_module_s: 模块的基本属性（基类）
- ctx_index: 给定该模块属于core, http, event, mail四者中的哪种
- index: 给定该模块在编译后的所有模块中的位置
- *ctx: 模块上下文，定义模块初始化过程中的阶段调用（里面都是函数指针）
- 回调函数