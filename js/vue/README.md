VUEJS projects
---
1. Structure
    - build： 配置文件
    - config: 项目配置
        - index.js: 
    - src
        - App.vue: 根模板+初始化容器【引入属性和方法】
        - main.js: 入口js文件，用于初始化Vue项目对象
        - assets: 静态文件
        - components：项目组件
        - router：路由
        - mixin：通用的js code，不同组件中通用
        - views: 视图文件
    - static： 静态
2. 数据和方法：
    - 数据对象通过引用传递给Vue实例的data参数
    - Vue实例的data对象和模板中的对应元素一一绑定（只有在Vue实例创建时赋值的变量才会绑定，后期加入的不会绑定）
    - Vue实例的public属性和方法都以`$`开头，暴露出来以作调用
3. 钩子(hooks)
    - 其中函数的this上下文指向Vue实例
    - 不同阶段触发的钩子，可在实例体内进行通过方法声明的方式实现
        - created: 
        - mounted:
        
4. 