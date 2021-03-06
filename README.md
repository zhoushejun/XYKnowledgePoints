XYKnowledgePoints
=================
---
### Block

#### 类型
* NSGlobalBlock：未引用外部变量,类似函数,位于text段,
* NSStackBlock：位于栈内存，函数返回后Block将无效；
* NSMallocBlock：位于堆内存。

arc下,栈里的block 会被copy到堆上,mrc下不会,如果不手动写copy autorelease,在方法结束的时候会被释放.修饰符不同,一个用__weak,一个用__block.

### 持续集成
#### 定义
持续集成是一种软件开发实践，即团队开发成员经常集成他们的工作，通常每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试)来验证，从而尽快地发现集成错误。许多团队发现这个过程可以大大减少集成的问题，让团队能够更快的开发内聚的软件。

#### 价值
1. 减少风险
2. 减少重复过程
3. 任何时间、任何地点生成可部署的软件
4. 增强项目的可见性
5. 建立团队对开发产品的信心

#### 要素
1. 统一的代码库
2. 自动构建
3. 自动测试
4. 每个人每天都要向代码库主干提交代码
5. 每次代码递交后都会在持续集成服务器上触发一次构建
6. 保证快速构建
7. 模拟生产环境的自动测试
8. 每个人都可以很容易的获取最新可执行的应用程序
9. 每个人都清楚正在发生的状况
10. 自动化的部署

#### 原则
1. 所有的开发人员需要在本地机器上做本地构建，然后再提交的版本控制库中，从而确保他们的变更不会导致持续集成失败。
2. 开发人员每天至少向版本控制库中提交一次代码。
3. 开发人员每天至少需要从版本控制库中更新一次代码到本地机器。
4. 需要有专门的集成服务器来执行集成构建,每天要执行多次构建。
5. 每次构建都要100%通过。
6. 每次构建都可以生成可发布的产品。
7. 修复失败的构建是优先级最高的事情。

---
### 二分查找
优点是比较次数少，查找速度快，平均性能好；其缺点是要求待查表为有序表，且插入删除困难。

```
// array 为一个排好序的数组
int binarySearch(int array[], int length, int target)  
{  
    if (length/2==0 && array[length/2] != target ) return -1;  
    if (array[length/2]==target) return length/2;  
    if (array[length/2]>target]) return binarySearch(array[],length/2,target);  
    if (array[length/2]<target]) return binarySearch(array[],length/2,length-length/2);  
}
```

---
### 多线程
进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动,进程是系统进行资源分配和调度的一个独立单位. 
线程（thread）是组成进程的子单元，操作系统的调度器可以对线程进行单独的调度。

* pthread 是线程的POSIX标准
* NSThread 是Objective-C对pthread的一个封装.

```
@interface FindMinMaxThread : NSThread

- (void)start;
- (BOOL)isFinished

```

* GCD(Grand Central Dispatch) 是一套低层API,提供了一种新的方法来进行并发程序编写.作为开发者可以将工作考虑为一个队列，而不是一堆线程.公开有 5个不同的队列: 运行在主线程中的 main queue，3个不同优先级的后台队列,以及一个优先级更低的后台队列(用于I/O).开发者还可以创建自定义队列:串行或者并行队列。
* NSOperationQueue 操作队列.有两种不同类型的队列:主队列和自定义队列.主队列运行在主线程之上,而自定义队列在后台执行.在两种类型中,这些队列所处理的任务都使用 NSOperation 的子类来表述.


---
### Run Loops
Run Loops 不是多线程,一个run loop就是一个事件处理的循环,用来不停的调度工作以及处理输入事件.将直接配合任务的执行,它提供了一种异步执行代码的机制.

1. 使用port或是自定义的input source来和其他线程进行通信
2. 在线程（非主线程）中使用timer
3. 使用 performSelector...系列（如performSelectorOnThread, ...）
4. 使用线程执行周期性工作


```
// 让一个一个线程进入"等待"状态,直到shouldKeepRunning == NO;
BOOL shouldKeepRunning = YES;	// global
while (shouldKeepRunning){
        [NSThread sleepForTimeInterval:0.5];
        [[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode beforeDate:[NSDate distantFuture]];
}
```

---
### Runtime
#### 概念
Objective-C runtime是一个实现Objective-C语言的C库.对象可以用C语言中的结构体表示,而方法(methods)可以用C函数实现.另外再加上了一些额外的特性.这些结构体和函数被runtime函数封装后，Objective-C程序员可以在程序运行时创建,检查,修改类,对象和它们的方法.

1. 动态创建类
2. 创建对象
3. 消息派发

---
### 排序
##### 快速排序(Quick sort)
是对冒泡排序的一种改进.由C. A. R. Hoare在1962年提出.它的基本思想是:通过一趟排序将要排序的数据分割成独立的两部分,其中一部分的所有数据都比另外一部分的所有数据都要小,然后再按此方法对这两部分数据分别进行快速排序,整个排序过程可以递归进行,以此达到整个数据变成有序序列.

```
voidQuickSort(inta[],intnumsize)/*a是整形数组，numsize是元素个数*/
{
    inti=0,j=numsize-1;
    intval=a[0];/*指定参考值val大小*/
    if(numsize>1)/*确保数组长度至少为2，否则无需排序*/
    {
        while(i<j)/*循环结束条件*/
        {
            /*从后向前搜索比val小的元素，找到后填到a[i]中并跳出循环*/
            for(;j>i;j--)
                if(a[j]<val)
                {
                    a[i]=a[j];
                    break;
                }
            /*从前往后搜索比val大的元素，找到后填到a[j]中并跳出循环*/
            for(;i<j;i++)
                if(a[i]>val)
                {
                    a[j]=a[i];
                    break;
                }
        }
        a[i]=val;/*将保存在val中的数放到a[i]中*/
        QuickSort(a,i);/*递归，对前i个数排序*/
        QuickSort(a+i+1,numsize-1-i);/*对i+1到numsize-1这numsize-1-i个数排序*/
    }
}
```
#### 冒泡排序(Bubble Sort)
重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。

---
### 面向对象
#### 三个基本特征
* 封装
* 继承
* 多态

#### 六个设计原则
1. 单一职责原则(Single-Resposibility Principle).不要存在多于一个导致类变更的原因。通俗的说，即一个类只负责一项职责。
2. 里氏替换原则(Liskov-Substituion Principle).所有引用基类的地方必须能透明地使用其子类的对象。
3. 依赖倒置原则(Dependecy-Inversion Principle).高层模块不应该依赖低层模块，二者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。
4. 接口隔离原则(Interface-Segregation Principle).客户端不应该依赖它不需要的接口；一个类对另一个类的依赖应该建立在最小的接口上。
5. 迪米特法则(Law of Demeter).一个对象应该对其他对象保持最少的了解。
6. 开放封闭原则(Open-Closed principle).一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。

#### 23个设计模式
1. 单例模式　
2. 工厂方法模式　
3. 抽象工厂模式　
4. 模版方法模式
5. 建造者模式
6. 代理模式
7. 原型模式
8. 中介者模式
9. 命令模式
10. 责任链模式
11. 装饰模式
12. 策略模式
13. 适配器模式
14. 迭代器模式
15. 组合模式
16. 观察者模式
17. 门面模式
18. 备忘录模式
19. 访问者模式
20. 状态模式
21. 解释器模式
22. 享元模式
23. 桥梁模式

#### 消息转发
消息转发分为两大阶段。

第一阶段先征询接收者，所属的类，看其是否能动态添加方法，以处理当前这个“未知的选择子”（unknown selector），这叫做“动态方法解析”（dynamic method resolution）。

第二阶段涉及“完整的消息转发机制”（full forwarding mechanism）。如果运行期系统已经把第一阶段执行完了，那么接收者自己就无法再以动态新增方法的手段来响应包含该选择子的消息了。此时，运行期系统会请求接收者以其他手段来处理与消息相关的方法调用。这又细分为两小步。

1. 首先，请接收者看看有没有其他对象能处理这条消息。若有，则运行期系统会把消息转给那个对象，于是消息转发过程结束，一切如常。
2. 若没有“备援的接收者”（replacement receiver），则启动完整的消息转发机制，运行期系统会把与消息有关的全部细节都封装到NSInvocation对象中，再给接收者最后一次机会，令其设法解决当前还未处理的这条消息。

大致流程:

* SEL -NO->
* resolveInstanceMethod -NO->
* forwardingTargetForSelector -NO->
* (methodSignatureForSelector) -> forwardInvocation -NO->
* Fail unrecognized selector send to instance 0xXXXX

### Socket
#### 服务器工作流程
1. 服务器调用 socket(...) 创建socket；
2. 服务器调用 listen(...) 设置缓冲区；
3. 服务器通过 accept(...)接受客户端请求建立连接；
4. 服务器与客户端建立连接之后，就可以通过 send(...)/receive(...)向客户端发送或从客户端接收数据；
5. 服务器调用 close 关闭 socket；

#### 客户端工作流程
1. 客户端调用 socket(...) 创建socket；
2. 客户端调用 connect(...) 向服务器发起连接请求以建立连接；
3. 客户端与服务器建立连接之后，就可以通过 send(...)/receive(...)向客户端发送或从客户端接收数据；
4. 客户端调用 close 关闭 socket；