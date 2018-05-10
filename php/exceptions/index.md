## 异常

### Catch
用于捕获异常
```$xslt
try{
    // do somethig
}catch(Exception $e){
    // deal with exception
}
```

### throw
用于遇到一些错误情况时，主动抛出异常
```$xslt
if(/*there is an error){
    throw new Exception("An Exception occured", 500);// 错误信息，错误码
}
```

### 多重catch
用于对可能出现的几个异常情况进行列举
```
try{
    // do something that results in multiple cases
}catch(ServerException $e){
    // deal with server exception
}catch(ClientException $e){
    // deal with client exception
}
```