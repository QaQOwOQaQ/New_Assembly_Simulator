## 一、新的开始 -- 重构旧汇编模拟器

### 9.22 
（1）在旧的汇编模拟器基础上进行了第一次重构，并且测试程序可以成功运行。

（2）一些相关函数还没有实现，因此程序会在执行完第一条指令后挂掉(coredump)。


### 9.23
（1）实现函数：string2uint_range()。

（2）实现函数：inst_parse()。

（3）实现函数：operand_parse()。

（4）优化了cpu的flag位，原本的实现是一个flag位占用一个uint64，一共需要4个uint64，现在通过struct和union只需要一个uint64就可以表示4个flag位。



### 9.24
（1）完成了堆汇编指令的解析，并分离出指令和操作数。

（2）经过调试和debug，程序可以成功测试汇编程序add。

（3）程序可以成功运行递归程序sum。

（4）至此，汇编模拟器的第一阶段----指令部分基本完成。


### 9.25
（1）修改函数：TestAddfunction（）。使得可以把程序指令存储在程序的内存当中而不是我们的汇编模拟器的内存当中。并且测试成功。

（2）现在，汇编模拟器的第一阶段----指令部分圆满完成。


## 二、静态链接
### 10.1
（1）对代码完成了一次小重构：增加了bin文件夹(放置生成的可执行文件)和test文件夹(放置测试程序)。

（2）成功将ELF文件的内容读取到我们开辟的内存空间buffer中。

### 10.3
（1）完成了对ELF文件的解析。

（2）完成了对重复符号的决议（符号解析三个规则）。

### 10.4
（1）静态链接部分和重定位部分相当迷糊，需要停下动态链接部分，接下来需要再对这两个部分加深印象。   
 
### 10.5
（1）对符号解析和静态链接重写，整体上对代码框架更加熟悉了。

（2）对重定位的相关内容还不太了解，需要回顾。

### 10.8
（1）添加了一些了数据结构(双向循环链包、动态数组、字典树、哈希表)和相关算法。

（2）对哈希表的实现不太清楚，其余三个大体熟悉了。

### 10.11
（1）完成了 PC 相对寻址和绝对寻址。到此为止，静态连接器就已经完成了。不过，我们的 call 指令只实现了 PC 绝对寻址，相对寻址的部分还没有实现。

（2）修改了 inst_t 的位置，放到了一个新文件 instruction.h。原本放在 isa.c 中无法被外部调用。

（3）动态链接不考虑。


## 三、内存
### 10.13
（1） 完成了 cache 的结构体创建以及两个接口（读/写 cache）的框架。

（2）[检测Cache-LRU正确性](https://www.bilibili.com/video/BV17K4y1N7Q2?p=30&vd_source=38033fe3a1f136728a1d6f8acf710b51)。视频前 40 分钟基本都在介绍如何检测 LRU 的正确性，通过利用 py 脚本和 CSPP Cache Lab 的测试数据。然而我并不太懂，关于测试这一部分以后有机会再说吧（大概率不会再看了T.T）。

（3）完成了 cache 的读/写实现。

### 10.14
（1）待完成：恢复成单核处理器。

（2）完成了 EMSI 协议：一个多线程（多cpu核心）下的控制指令的执行顺序和 cache 状态的改变以保证 **数据一致性** 的协议。真的是历经千辛万苦，写出重重 bug，克服重重 bug。（bug log 详见 mesi.c 文件末尾）

### 10.15
（1）完成了 TrueSharing，FalseSharing，NoSharing 的效率比较（test_false_sharing.c）,但是测试结果不太正确，可能存在一些优化？？不过无关紧要，只需要了解 FalseSharing 导致低效率的特殊情况即可。那么，cache 部分终于完结了。接来下是多级页表和地址翻译部分。

（2）修改 mmu 中的 va2pa 函数。不再使用取模的方法，而是更合理的页表。

（3）实现了 Linux 四级页表地址翻译的相关数据结构，对页表的地址翻译的原理还不太清晰，指令集部分的代码重构要提上日程了。


### 10.16
（1）完成 page_walk()和地址翻译 va2pa()。

