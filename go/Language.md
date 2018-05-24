## Go语言基础

### 特性
- struct
    - 定义一个struct:
    ```
    type Test struct {
        Id int
        Name string
    } //直接分行声明成员，不用逗号分割

    ```
    - 定义一个struct函数:
    ```
    func (t *Test) PUT() {
        fmt.Println(t.Id, t.Name)
    }
    ```
    
    - 声明一个对象
    ```
    test_obj := &Test{
        Id: 1,
        Name: "test",
    }//声明对象，成员间需要逗号分割, 且最后一个元素也要逗号
    ```
    
   - 包含一个struct(实际上可以当作继承，只不过这里通过组合的方式， 且父类的函数可以直接调用)
   ```
   type TestSon struct {
        Test // 匿名成员，用作继承(包含)
        Extra string
   }
   ```
   - 声明一个包含对象
   ```
   t := &TestSon{
        Test : Test{
            Id: 1,
            Name: "f",
        },
        Extra "fff",
   }
   t.PUT() //PUT是父类Test的成员函数
   ```
   
  