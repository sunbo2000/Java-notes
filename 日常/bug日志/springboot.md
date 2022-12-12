### 1.required a bean of...

有些时候编译时没有报错,运行时出现 :

`required a bean of type 'org.clouddemo.common.feign.Demo2Client' that could not be found.`

spring 没有找到说明运行时容器里没有这个对象,也就是说 spring 没有扫描到这个类,没有创建这个对象

这时应该去查看是不是这个类输入公共模块,在其他模块引入了公共模块但是没有指定包扫描范围, spring 默认扫描启动类所在的包及其以下,导入公共模块后需要指定扫描范围