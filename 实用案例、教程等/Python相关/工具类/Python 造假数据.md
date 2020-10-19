### **1、mimesis**



这个库专门用 Python 创建各种假数据，比如一些数据库的测试数据，假 API、Json、XML 等格式数据都可以通过它产生，把假数据整的像真的似得。

**文档在这：**

https://mimesis.readthedocs.io/api.html

而且支持 33 个不同地方的本地语言假数据生成：

![img](.Python 造假数据.assets/640-1602822749880.png)

来带你体验一波：

首先导入 mimesis 的 Person 对象：

![image-20201016132153692](.Python 造假数据.assets/image-20201016132153692.png)

接着定义中文，使用 pprint 将这个对象给打印出来：

![image-20201016132212629](.Python 造假数据.assets/image-20201016132212629.png)

运行一下就可以看到 Person 对象定义的各种假数据了，随便截几张图给你体会一下。



学位、性别、语言：

![image-20201016132228560](.Python 造假数据.assets/image-20201016132228560.png)

名称：

![image-20201016132302328](.Python 造假数据.assets/image-20201016132302328.png)



甚至还有**性取向** 和 姓氏：

![img](.Python 造假数据.assets/640.webp)

​        

学士学位、就读大学：

![image-20201016132330937](.Python 造假数据.assets/image-20201016132330937.png)



还有很多其它信息就不一一举例了，除了 Person 之外，还有 food、 address、transport、Business 等对象提供的相应假数据。

![image-20201016132344523](.Python 造假数据.assets/image-20201016132344523.png)





实际上，当你需要用到相关的假数据的时候，你只需要调用相关的对象方法即可：

![image-20201016132406191](.Python 造假数据.assets/image-20201016132406191.png)



另一个比较方便的就是 API 假数据的创建，你可以先创建一个 py 文件，在里面使用 mimesis.schema 定义要返回的 Json 参数数据格式：

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![image-20201016132421200](.Python 造假数据.assets/image-20201016132421200.png)



接着在你需要返回 API 的方法中调用它即可：

![image-20201016132435560](.Python 造假数据.assets/image-20201016132435560.png)



这样调用这个接口你就可以得到相关的假数据啦：

![image-20201016132447759](.Python 造假数据.assets/image-20201016132447759.png)











### **2、fake2db**

**代码在这：**

https://github.com/emirozer/fake2db

另一个常需要用到假数据的就是数据库了，fake2db 这个库可以给数据库填充假数据，它可以支持我们常用到的数据库，比如 MySQL、Redis、Mongodb、Sqlite 等。



安装完 fake2db 之后，你就可以使用它的命令来生成假数据了：

![img](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)![image-20201016132541348](.Python 造假数据.assets/image-20201016132541348.png)

比如你要创建一个 Sqlite，填充 10 条假数据就可以这样：

> fake2db --rows 10 --db sqlite

![image-20201016132601253](.Python 造假数据.assets/image-20201016132601253.png)



可以看到，这里帮我们创建了 sqlite_QPNVJVIX.db， 并且生成了多张数据表，我们进去看一下：

![image-20201016132619329](.Python 造假数据.assets/image-20201016132619329.png)



查询一下 user_agent 表中的数据：

![image-20201016132633503](.Python 造假数据.assets/image-20201016132633503.png)

可以看到这里有 10 条假 user_agent 数据。



注册信息：

![image-20201016132648050](.Python 造假数据.assets/image-20201016132648050.png)



你要多少数据都可以，只要把刚刚的命令中的 --rows 参数设置大一点就可以了。



可能有些数据表的字段你想自己定义，那么可以在 fake2db 的 custom.py 中先定义好字段参数：

![img](.Python 造假数据.assets/640-1602826026817.png)



比如我想生成一张含有 user_name、password、email、date 的 Sqlite 数据表，然后往里填充 100 条假数据，就可以这样：

![image-20201016132725754](.Python 造假数据.assets/image-20201016132725754.png)



这里使用 --custom 将你要自定义的字段参数传进来就可以了，这时候生成的表数据就是你定义的样子：

![img](.Python 造假数据.assets/640.png)







以后需要测试数据再发不用发愁了。