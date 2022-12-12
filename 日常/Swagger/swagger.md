## dataType 和 paramType

dataType="int" 代表请求参数类型为int类型，当然也可以是Map、User、String等；

paramType="body" 代表参数应该放在请求的什么地方：

    header-->放在请求头。请求参数的获取：@RequestHeader(代码中接收注解)
    query-->用于get请求的参数拼接。请求参数的获取：@RequestParam(代码中接收注解)
    path（用于restful接口）-->请求参数的获取：@PathVariable(代码中接收注解)
    body-->放在请求体。请求参数的获取：@RequestBody(代码中接收注解)
    form（不常用）

<img src="D:\A.学习资料\A.笔记\Swagger\swagger.assets\image-20220723162131729.png" alt="image-20220723162131729" style="zoom:80%;" />