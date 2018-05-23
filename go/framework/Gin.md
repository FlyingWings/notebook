## GIN

### 特性
- 快
- 小
- 有日志+回复的中间件，功能完善

### 样例
```
import (
	"github.com/gin-gonic/gin"
)
func main() {
    router := gin.Default() // 这里用的是默认配置（加载了两个中间件）
    router.GET("/ping", func(c *gin.Context) {
        c.String("pong")  //返回字符串
    }
    
    router.Run(":8080") //默认在8080上运行
```

### 结构解析
- gin.Default()
    - `func Default() *Engine`: 返回引擎的类型的变量
    - `engine := New()`: 新建一个引擎——>在New()里面解析
    - `engine.Use(Logger(), Recovery())`： 启用日志+恢复中间件
    - `return engine`
    
    
