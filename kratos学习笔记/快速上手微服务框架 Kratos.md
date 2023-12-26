[TOC]

# 快速上手微服务框架 Kratos

## 把官方的 demo 跑起来

读官方文档的快速开始，然后安装依赖，把官方的 helloworld 的 demo 跑起来

官方文档地址：[Kratos (go-kratos.dev)](https://go-kratos.dev/)

## demo 的架构简单分析

![image-20231226103332725](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226103332725.png)

先打开 api 文件，看这里的 greater.proto

![image-20231226103444628](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226103444628.png)

proto 文件也被称为 go 程序员的接口文档（听一个学 go 的哥们说的），从这里我们就能看到这里启动了一个 http 的服务，跟着官方文档的指引，我们把项目跑起来，可以去浏览器访问一下，不过我习惯用 ApiPost 来调试代码：

![image-20231226104150919](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226104150919.png)

好吧，来都来了，浏览器也跑一下玩玩：

![image-20231226104221603](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226104221603.png)

回到正题，继续看代码：

![image-20231226104315656](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226104315656.png)

cmd 就是程序启动的地方，wire 我还没仔细看，暂时不重要，主要还是看 main.go 文件

![image-20231226104451983](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226104451983.png)

简单来说就是加载配置，然后最后 app.Run() 跑起来，代码还是很清晰的。

然后 configs 文件就不用说了，放的就是 yaml 文件，配置信息

![image-20231226104906255](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226104906255.png)

internel 就是放的不暴露到外面的代码，说人话就是具体的代码逻辑就是写在这里

**biz**

业务逻辑的组装层，类似 DDD 的 domain 层，data 类似 DDD 的 repo，repo 接口在这里定义

**data**

业务数据访问，包含 cache、db 等封装，实现了 biz 的 repo 接口

**service**

实现了 api 定义的服务层，类似 DDD 的 application 层，处理 DTO 到 biz 领域实体的转换（DTO -> DO），同时协同各类 biz 交互，但是不应处理复杂逻辑

**server**

为 http 和 grpc 实例的创建和配置，以及注册对应的 service 

**conf**

这里面比较特殊，我也是第一次见这种操作，用 proto 来写配置信息，然后直接生成 go 代码

**总结**

这里讲了一大堆其实没什么用，大概知道一下就行了，最后怎么分包，怎么实现还是看自己想怎么做，喜欢怎么做，这里只是作为一个参考，go 并不像 Java 那么注重一模一样的规范

![image-20231226105831270](https://formyblog-1316637577.cos.ap-guangzhou.myqcloud.com/img/image-20231226105831270.png)

最后就是 third_party，这个就是第三方库，说到这里，补充一句，如果你用的是 GoLand，然后发现你的 proto 文件的这个：import "google/api/annotations.proto"; 爆红了，但是不影响编译，用这个教程可以消去爆红：[kratos goland google/api/annotations.proto 标红_goland中proto报红-CSDN博客](https://blog.csdn.net/brazor/article/details/124455504)



然后官方的这个 demo 也给我们提供了 Dockerfile，准备了 Makefile 等等这些就不细说了，等用上了再说，如果没用上刚好就不用说了，少说废话，多实战才是真的。

