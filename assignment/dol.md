# dol

标签（空格分隔）： 嵌入式 dol

---

## 改完后的dol截图
- example2
square模块由3个变成了两个
![example2.dot][1]

- example1
由于是在square模块里进行更改所以dot图不变
![example1.dot][2]

## 如何修改
- example2
打开example2.xml，找到
``<variable value="3" name="N"/>``
并修改为
``<variable value="2" name="N"/>``
这句话的意思相当于c语言里的宏定义，由于下面的代码迭代次数都为N，所以只要把N的宏定义由3改为2就可以让3个square模块变为2个。

- example1
打开square.c， 找到square_fire函数，把里面的
``i = i*i;``
改为
``i = i*i*i;``
i为square模块读入的generator生成的数，然后对i进行立方处理，最后输出到consumer模块里。

## 实验感想
还没上实验课之前对dol一头雾水，但经过仔细研究example1和2以及TA详细的讲解之后初步了解了dol的工作过程。
- .c与对应的.h是实现的模块，每个模块都有xxx_init和xxx_fire两个函数，前者是初始化函数，后者是信号产生函数，可以接收和输出数据，中间可以对数据进行处理。
- .xml看上去比.c和.h文件复杂。可分为3部分。process部分定义模块以及端口名称；sw_channel部分定义通道，即dot文件里把两个模块连接起来的通道；connection部分定义各个模块之间的连接，需要注意的是，一条线（通道）会对应两个connection，即模块与模块之间不能越过通道直接相连。
- 学习example2里的.xml文件可知迭代的格式为
```
<iterator variable="i" range="N">
    <!-- 迭代内容 -->
</iterator>
```

  [1]: http://ww4.sinaimg.cn/large/69347328jw1f8oou38sm5j20e90f10tc.jpg
  [2]: http://ww3.sinaimg.cn/large/69347328jw1f8ooxhnzjqj20ea0ex0tc.jpg