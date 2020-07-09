Lua语言特性
---
#### 注释
- 单行: -- 注释内容
- 多行: 
--[[
注释内容
--]]
#### 变量
- variable = nil -- 用来删除变量
- 默认都是全局变量，只有local关键词修饰的是局部
#### 字符串：单引号，双引号，双中括号作为多行
```lua
s = "1"
v = 'b'
q = [[ TEST 
]]
```
#### 循环
    - for - do - end
    - while - do - end
    - repeat until
    
#### 函数
    - function - end
    - 多返回值

#### 数据结构：Table 等类似PHP的中array
    - 定义：`t = { k = v, k2 = v2}`
    - 调用成员: t.k
    - unset: t.k = nil
    - key可以是任何类型，但是不作死的话就用数字和字符串（别的类型很难对应到key）
#### 元表+元方法
- 可以理解为是对table进行一层扩展，定义一个特殊的table类型（元表），并可以为其增加特定方法
```lua
metaTable = {}
function metaTable.__add(m1, m2)  -- 定义元方法
   -- 执行
end
v1 = {}
v2 = {} 
setmetatable(v1, metaTable) -- 声明元表
setmetatable(v2, metaTable)
v1 + v2 -- 操作符重载
```
- 元方法列表
   
   
    - __add(a, b)                     for a + b
    - __sub(a, b)                     for a - b
    - __mul(a, b)                     for a * b
    - __div(a, b)                     for a / b
    - __mod(a, b)                     for a % b
    - __pow(a, b)                     for a ^ b
    - __unm(a)                        for -a
    - __concat(a, b)                  for a .. b
    - __len(a)                        for #a
    - __eq(a, b)                      for a == b
    - __lt(a, b)                      for a < b
    - __le(a, b)                      for a <= b
    - __index(a, b)  <fn or a table>  for a.b
    - __newindex(a, b, c)             for a.b = c
    - __call(a, ...)                  for a(...)
#### OO语法（基于原型的语法，类似JS）
- 声明元类表
- 注明元方法
