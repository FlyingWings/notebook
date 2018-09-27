# GO-KIT 微服务入门

- 基本概念
    - 入口文件
    - 路由调度
    - 服务分发
    - 监听请求
    - 返回响应
    - 中间件

- 入口文件
    - 每个服务的入口main文件，里面包含一系列包引用和路由等规则
    
- 路由调度
    - 定义：由http模块所处理的请求-地址对应的路由部分，如
    ```
    http.Handle("/index", models.StatHandler) // 定义URI： index， 并用StatHandler处理
    ```
    
- 微服务结构
    - 服务接口：用于定义函数原型，声明整个服务结构
   ```
   type StringService interface {
        ReverseString(context.Context, string) (string, error)
   // 声明一个服务的原型，里面一个方法，这个方法就是即将被调用的
   ```
   - 服务结构：用于定义服务对象，以及作为方法的载体
   ```
   type stringService struct {}
   // 空结构体，用于承载方法
   ```
   - 请求和响应结构：用于声明请求数据和响应数据的数据结构
   ```
   type stringRequest struct {
        S string `json:"s"`
   }
   // 请求格式，key为s的字符串
   
   type stringResponse struct {
        Res string `json:"res"`
        Err string `json:"err,omitempty"`
   }
   // 响应格式，其中错误码key为err，若空则省略
   ```
   - 服务方法主体：用于定义方法功能（主体）
   ```
   func (stringService) ReverseString( _ context.Context, s string) (string, error) {
        return s, nil // 处理请求数据后返回
   }
   ```
   - 定义Endpoint：用于声明访问的终点
   ```
   func makeStringEndpoint(svc StringService) endpoint.Endpoint {
        return func(ctx context.Context, req interface{}) (interface{}, error) {
            v, _ := svc.ReverseString(ctx, req.S)
            return stringResponse{v, nil}, nil
        }
   }
   ```
   - Handler声明
   ```
   var StatStringHandler *http.Server
   func init(){ 
        svc := stringService() // 定义服务
        StatStringHandler = http.NewServer(
            makeStringEndpoint(svc),
            DecodeRequest,
            EncodeResponse,
        )
   }
   ```
   
   