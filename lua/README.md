Lua语言特性
---
1. 标准库功能集成，扩展性强
2. 胶水语言

### 类型
- nil
- boolean
- number
- string
- userdata
- function
- thread
- table
##### 使用方法type(xx) 获取类型描述，返回一个string
#### string
1. 不可变值（给定边界的字符组空间）
2. 类型转换函数 `tonumber()` 和 `tostring()`
3. 语法糖: `#` 用于获取字符串长度
#### table
1. 通用数据结构，用于数组，模块，包，对象等
2. 创建方式：构造表达式`{}`,并将表达式的引用赋值给某个变量
3. 你永远无法手动干掉一个table，只能通过unset所有引用的方式，触发垃圾回收
4. 调用和赋值方式：`a["key"], a.key （后者实际上key是一个string）
5. 语法糖：`#` 用于获取table长度
6. table的用法
    - 插入：a[#a+1] = newObj
    - 
#### function
函数


### 语句
1. 赋值：支持多重赋值
2. 尽可能使用local变量
3. 控制结构
    - if then elseif then else
    - while do end
    - for start,end,stp_len do end
### 函数
1. 实际上都是函数指针，特殊的数据结构
2. 语法糖: `:`，利用该方法规避self传值
3. unpack特性：将某个table拆分多个返回值（类似于解构调用)
```lua
a, b, c = unpack(10, 20, 30)
```
4. 变长参数：用于获取实参中所有的参数，并且在函数体里面也用...
```lua
function getValue(v1, v2, ...)
    local t, s = ...
    -- 例如这种
end
```
5. 局部函数
6. 尾调用：A函数调用B函数后直接返回的情况，不需要为这次调用保存栈信息