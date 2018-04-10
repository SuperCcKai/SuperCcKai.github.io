---
layout: post
title: java基础性问题
date: 2017-10-21
tags: problem
---


### 为什么数据结构中顺序表是随机存取的而链表不是

顺序表可以随便访问任何一个元素，像C中，我要访问数组a中第三个元素就a[2]。单纯的地址加操作而已。
链表就必须从第一个开始一个一个遍历，最后才能找到第三个。


### 多次调用Scanner出错

在一个工程中使用不同的Scanner对象，运行时程序会报错："java.util.NoSuchElementException".

这一原因是`close()`关闭了scanner。解决方法是删除掉过多的`sc.close()`，只在main函数的最后保留一个close().


### Eclipse中的web项目如何自动部署到tomcat的webapp目录下

在Eclipse中做的Web项目默认是不支持将项目发布到Web服务器上的，会发布到工作空间的某个目录，因此无法在外部启动Tomcat来运行Web项目，只有打开Eclipse中的服务器，才能运行Web项目。所以要对Eclipse进行修改，才能将做好的项目，发布到Tomcat服务器上，发布到服务器上的Webapps文件夹下。

在Eclipse中，默认会把Web项目放到Eclipse的当前工作空间下的.metadata\.plugins\org.eclipse.wst.server.core\tmp0(或者是tmp1)\wtpwebapps\下

解决办法：

1.停止Ecclipse内运行的web项目，并将其移除；

2.双击或者右键"open"Eclipse底下的Servers视图内的tomcat服务器；

3.在“Server Locations”处选择“Use Tomcat installation”，然后在Deploy的path处写上webapps即可；此处默认是“Use Workspace metadata”，即上边所说的目录；

4.ctrl+s保存。

注意：

1. 在Eclipse中，用这种方式发布Web项目，会将原先服务器的conf文件夹被分成为backup文件夹，自己新建立一个文件夹，来作为配置Eclipse发布的Web项目。所以还是要使用MyEclipse编写Java Web项目

2. 有时候，server Locations项目下，什么都不可选择，你可以先删除服务器，重新添加tomcat服务器，然后右击打开，修改即可。


### 关于类中的默认函数

父类显式创建了有参构造函数，那么就不会再给它提供默认的无参构造函数了


### 并发与并行的简单区分

一皇二后是并发，二皇一后是并行。


### 方法的重写原则

方法的重写（override）两同两小一大原则： 

>方法名相同，参数类型相同 

>子类返回类型小于等于父类方法返回类型， 

>子类抛出异常小于等于父类方法抛出异常， 

>子类访问权限大于等于父类方法访问权限。


### java多线程

Java中的多线程是一种抢占式的机制，而不是分时机制。抢占式的机制是有多个线程处于可运行状态，但是只有一个线程在运行。

共同点 ： 

1. 他们都是在多线程的环境下，都可以在程序的调用处阻塞指定的毫秒数，并返回。 

2. wait()和sleep()都可以通过interrupt()方法 打断线程的暂停状态 ，从而使线程立刻抛出InterruptedException。 

如果线程A希望立即结束线程B，则可以对线程B对应的Thread实例调用interrupt方法。如果此刻线程B正在wait/sleep/join，则线程B会立刻抛出InterruptedException，在catch() {} 中直接return即可安全地结束线程。 

需要注意的是，InterruptedException是线程自己从内部抛出的，并不是interrupt()方法抛出的。对某一线程调用 interrupt()时，如果该线程正在执行普通的代码，那么该线程根本就不会抛出InterruptedException。但是，一旦该线程进入到 wait()/sleep()/join()后，就会立刻抛出InterruptedException 。 

不同点 ：
  
1. 每个对象都有一个锁来控制同步访问。Synchronized关键字可以和对象的锁交互，来实现线程的同步。 
sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。 

2. wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用 

3. sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常  

4. sleep是线程类（Thread）的方法，导致此线程暂停执行指定时间，给执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复。调用sleep不会释放对象锁。 

5. wait是Object类的方法，对此对象调用wait方法导致本线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象发出notify方法（或notifyAll）后本线程才进入对象锁定池准备获得对象锁进入运行状态。


### return i++;

return i++, 先返回i，然后i+1；


### super关键字

super是java提供的一个关键字，super用于限定该对象调用它从父类继承得到的Field或方法。super关键字不能出现在static修饰的方法中，因为static修饰的方法是属于类的。如果在构造器中使用super，则super用于限定该构造器初始化的是该对象从父类继承得到的field，而不是该类自己定义的field。需要注意的是，super关键字只能指代直接父类，不能指代父类的父类。 


### 比较器是什么意思，求解

比较器就是一个接口，两个对象要使用compareTo方法比较大小，就必须实现Comparator接口的compare方法，比如String就实现了这个方法，所以可以直接使用compareTo进行比较。sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序。通过实现这个接口重写compare方法，返回正值代表大于返回0代表等于，返回负值代表小于。这样就可以自定义排序方法。jdk8的List本身也支持比较器排序。jdk7中集合可以使用Collections的sort方法，实现自定义比较器排序，数组可以通过Arrays.sort()方法实现自定义比较器排序。


### 类成员变量

类成员变量在实例方法中能用this去调用，而在静态方法中不能用this去调用。


### 类的加载顺序

首先，需要明白类的加载顺序。

(1) 父类静态对象和静态代码块

(2) 子类静态对象和静态代码块

(3) 父类非静态对象和非静态代码块

(4) 父类构造函数

(5) 子类 非静态对象和非静态代码块

(6) 子类构造函数

其中：类中静态块按照声明顺序执行，并且(1)和(2)不需要调用new类实例的时候就执行了(意思就是在类加载到方法区的时候执行的)

因为静态成员变量和静态代码块的优先级是相同的，所以，先定义的先执行，如果把静态代码块放在静态成员前面输出就是BAA。可以自己试一下


### button1.addActionListener(this);

swing

`button1.addActionListener(this);`

就是把当前的类设成button1的Listener;

那么就可以在当前的类中添加方法以响应button1的点击,比如:

```
void actionPerformed(ActionEvent e)
{
//响应button1的点击的代码;
}
```


### Java中每当创建一个窗口的时候为什么要在外面写EventQueue.invokeLater

我今天也遇见了这个问题，从网上找了一下答案。其中有一个答案是这样的：
Java's GUI is strictly single-threaded.
All GUI related things in java should always go through a single thread. The thread is our legendary "AWT-EventQueue-0"   . Hence all GUI related actions should necessarily go through the AWT Event thread. If not so you may end up in a deadlock. For small programs, this might never happen. But for a huge java application if you try frame.setVisible(true) kind of thing in main thread, you will soon find yourself searching a new job. What invokeLater() does is to post your Runnable in the AWT thread's event queue. So the code in your run method will be executed in the AWT-Eventqueue thread.
大意是说，java的GUI都是的单线程，应该使用事件调度线程去执行，如果没意思使用事件调度线程的话，可能造成死锁。但是在小的程序中，这种现象（死锁）不会发生的；大的应用程序中才会出现这种现象!


### java的getSource()方法

swing

ActionEvent 的 getSource() 继承自 EventObject；

public Object getSource()

返回：最初发生 Event 的对象。

例子中,把if(e.getSource()==btn) 去掉可行，因为你的例子中只有一个button。

如果你有两个button，还有一个btn2,且都addActionListener的话。

必须要用上e.getSource()来区分响应哪个button了

```
if(e.getSource()==btn)
 //todo btn
else if(e.getSource()==btn2)
 //todo btn
```

e.getSource()返回的是Object,将Object强制转换为Button试试


### java的getClass()方法

利用这个方法就可以获得一个实例的类型类。类型类指的是代表一个类型的类，因为一切皆是对象，类型也不例外，在Java使用类型类来表示一个类型

```
A a = new A();
if(a.getClass()==A.class)
	System.out.println("equal");
else 
	System.out.println("unequal");
```


### java的charAt()怎么用

```
public class Test {
    public static void main(String[] args) {
        String s ="abc";
        System.out.println(s.charAt(1));
    }
}
```

the answer is: b


### java的KeyEvent的keyPressed和keyTyped有什么区别

swing

API：

例如，按下 Shift 键会生成 keyCode 为 VK_SHIFT 的 KEY_PRESSED 事件，而按下 'a' 键将生成 keyCode 为 VK_A 的 KEY_PRESSED 事件。

释放 'a' 键后，会激发 keyCode 为 VK_A 的 KEY_RELEASED 事件。

另外，还会生成一个 keyChar 值为 'A' 的 KEY_TYPED 事件。

`void keyPressed(KeyEvent e)`
按下某个键时调用此方法。 

`void keyReleased(KeyEvent e)`
释放某个键时调用此方法。 

`void keyTyped(KeyEvent e)`
键入某个键时调用此方法。 


### GridBagConstraints参数详解 

swing

```
gridx = 2; // X2
gridy = 0; // Y0
gridwidth = 1; // 横占一个单元格
gridheight = 1; // 列占一个单元格
weightx = 0.0; // 当窗口放大时，长度不变
weighty = 0.0; // 当窗口放大时，高度不变
anchor = GridBagConstraints.NORTH; // 当组件没有空间大时，使组件处在北部
fill = GridBagConstraints.BOTH; // 当格子有剩余空间时，填充空间
insert = new Insets(0, 0, 0, 0); // 组件彼此的间距
ipadx = 0; // 组件内部填充空间，即给组件的最小宽度添加多大的空间
ipady = 0; // 组件内部填充空间，即给组件的最小高度添加多大的空间
new GridBagConstraints(gridx, gridy, gridwidth, gridheight, weightx, weighty, anchor, fill, insert, ipadx, ipady);
gridBagConstraints.insets = new java.awt.Insets(4, 4, 0, 4);//设置组件的位置
```
 
Insets是AWT里面一个类的名字，代表着类Insets，它的用途是用来定义组件容器周围的空间大小，其中带有四个参数：

`Insets(第一个参数，第二个参数，第三个参数，第四个参数 )`

第一个参数代表距上面有几个点的空白，第二个参数代表距左边有几个点的空白，第三个参数代表距下边有几个点的空白区域，第四个参数代表距右边留几个点的空白区域。


### java的setResizable(boolean)的含义

swing

方法setsetResizable（boolean resizable）

参数为boolean类型，resizeable值为true时，表示在生成的窗体可以自由改变大小；

resizeable值为false时，表示生成的窗体大小是由程序员决定的，用户不可以自由改变该窗体的大小。


### Java中menu.addSeparator 是什么意思 

swing

这个是 菜单中横线分隔符号  就是在 两个菜单项的

例如 你打开 我的文件夹 中的 上边的下拉菜单 中就能看见 有条横线


### Swing-setBounds()用法

先看API：

public void setBounds(Rectangle r)

移动组件并调整其大小，使其符合新的有界矩形 r。由 r.x 和 r.y 指定组件的新位置，由 r.width 和 r.height 指定组件的新大小 

参数： r - 此组件的新的有界矩形

从API来看，该方法的作用相当于setLocation()与 setSize()的总和。

在实际使用时，需将容器的layout设置为null，因为使用布局管理器时，控件的位置与尺寸是由布局管理器来分配的。

需要注意的是，这时必须手动指定容器的尺寸，因为空的布局管理器会将容器自身的PreferredSize清零，导致容器无法在GUI上显示。

因此，如果容器在上级容器中使用布局管理器排列，那么需使用setPreferredSize()，如果容器在上级容器中仍然手动排列，那么对容器使用setBounds()即可。


### Java 关于paintComponent()与paint()

1 - paint() 中调用 paintComponent(), paintBorder(), paintChildren()；这三个方法一个是绘制背景，一个绘制边框，一个绘制子控件。
一般重写背景，是建议重写paintComponent()的。 

2 - 最重要的区别是“双缓冲”。Swing 组件的 paint() 中实现了双缓冲，所以不要随便去覆写，会破坏双缓冲的，————建议的方式是覆写 paintComponent()，很多人做的小程序会”闪烁“，就是因为他们覆写了 paint() 方法，破坏了Swing本身的双缓冲。Swing 不建议用户自己实现双缓冲。

3 - 覆写 paint()，如果新方法没有去调用 paintChildren()，还会造成子控件不显示，鼠标移上去才显示，这个也是很多新手问的问题： “为什么我的按钮只有鼠标移上去才显示？”

4 - 只有极少数的情况可能需要覆写 paint() 方法，通常是为了实现特殊的绘图效果，或者特殊的优化，比如 JViewport 覆写了 paint() 方法，使用“延迟重绘”的方式来合并当滚动条移动时一些特别频繁的重绘请求，等等。


### java可以往JPanel中添加一个JPanel吗？ 

可以的。方法从Component继承来的 


### paintComponent(Graphics g)方法什么时候被调用?

如果我们不重写paint(Graphics g)方法，那么会依照这个顺序调用，paintComponent，paintBorder，paintChildren。

也就是说如果我们想在窗口上绘制什么，但是又不想影响他上面的其他组件，可以在这个方法绘制。

如果我们重写了paint(Graphics g)方法，那么绘图全过程就被程序员接管了。


### eclipse如何重命名包名或java文件名

点击包名或java文件名，F2就可以了


### TreeSet怎么使用

TreeSet可以给Set集合中的元素进行指定方式的排序。保证元素唯一性的方式：通过比较的结果是否为0.底层数据结构是：二叉树。

**排序的第一种方式**： 让元素自身具备比较性。只要让元素实现Comparable接口，覆盖compareTo方法即可。

但是，如果元素自身不具备比较性，或者元素自身具备的比较性，不是所需要的。

比如，学生的自然排序是按年龄排序，现在想要按照学生姓名排序。还可以不改动原有代码。 这时怎么办呢？

**排序的第二种方式**：自定比较器的方式。这时可以让集合自身具备比较性。 

可以定义一个类实现Comparator接口，覆盖compare方法。

将该Comparator接口子类对象作为实际参数传递给TreeSet集合构造函数。 


### Collection与Map

Collection 

-----List

-----LinkedList    非同步

----ArrayList      非同步，实现了可变大小的元素数组

----Vector          同步

--------Stack

-----Set   不允许有相同的元素

Map 

-----HashTable        同步，实现一个key--value映射的哈希表，不可以接收null作为key或者value

-----HashMap          非同步，可以接收null作为key或者value

-----WeakHashMap   改进的HashMap，实现了“弱引用”，如果一个key不被引用，则被GC回收


### try-catch-finally 规则( 异常处理语句的语法规则 ） 

1)  必须在 try 之后添加 catch 或 finally 块。try 块后可同时接 catch 和 finally 块，但至少有一个块。 

2) 必须遵循块顺序：若代码同时使用 catch 和 finally 块，则必须将 catch 块放在 try 块之后。 
3) catch 块与相应的异常类的类型相关。 
4) 一个 try 块可能有多个 catch 块。若如此，则执行第一个匹配块。即Java虚拟机会把实际抛出的异常对象依次和各个catch代码块声明的异常类型匹配，如果异常对象为某个异常类型或 其子类的实例，就执行这个catch代码块，不会再执行其他的 catch代码块 
5) 可嵌套 try-catch-finally 结构。 
6) 在 try-catch-finally 结构中，可重新抛出异常。 

7) 除了下列情况，总将执行 finally 做为结束： JVM 过早终止（调用 System.exit(int)）；在 finally 块中抛出一个未处理的异常；计算机断电、失火、或遭遇病毒攻击 

由此可以看出，catch只会匹配一个，因为只要匹配了一个，虚拟机就会使整个语句退出 


### Eclipse 快捷键

自动修正: Ctrl + 1

代码提示: Alt + / (eg: main, sysout)

将一段代码统一向左移（右移）：shift + Tab (右移：Tab)

格式化代码: ctrl + shift + f

自动导入包: ctrl + shift + o

注释: ctrl + /, ctrl + shift + /, ctrl + shift + \

代码上下移动: 选中代码alt + 上/下箭头

查看源码:  选中类名(F3或者Ctrl+鼠标点击)

无参构造方法 在代码区域右键--source--Generate Constructors from Superclass   // alt+shift+s +c

带参构造方法 在代码区域右键--source--Generate Constructors using fields.. --finish  //alt+shift+s +o

自动生成get/set方法: 在代码区域右键--source--Generate Getters and Setters...  //alt+shift+s +r

source: alt+shift+s

顶端注释信息: /** + Enter

自动补全等式的前面部分，例如`list.iterator();`变为`Iterator<Element> iterator = list.iterator();`: 
    ctrl+2 松开 l


### String.intern()方法

java语言并不要求常量一定只有编译期才能产生，也就是并非预置入Class文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，即常量池的扩充，利用的比较多的便是String类的intern()方法。

当一个String实例str调用intern()方法时，java查找常量池中是否有相同unicode的字符串常量，如果有，则返回其引用；如果没有，则在常量池中增加一个unicode等于str的字符串并返回它的引用。下面为演示代码。

```
String s0 = "intern"; //"intern"进入常量池，s0为其引用
String s1 = new String("intern");
String s2 = new String("intern");
System.out.println(s0 == s1);
System.out.println(s0 == s1.intern()); //s1.intern()返回常量池中"intern"的引用
System.out.println(s0 == s2);
```

结果显示为：

```
false
true //s1.intern()，执行后等同于常量池中"intern"的引用，等于s0
false
```

另外，看如下例子：

```
String s1 = "java";
String s2 = "blog";
String s= s1+s2;
System.out.println(s == "javablog");
```

结果显示为：

```
false
```

jvm确实对形如`String str = "java";`的String对象放在常量池里，但是它是在编译时那么做的，而`String s = str1+str2;`是在运行时才能知道，也就是说`str1+str2`是在堆里创建的，所以结果为`false`；

最后注意，常量池中一开始就存在"java";


### java泛型总结1

泛型的好处：1.把运行时出现的问题提前至了编译时。2.避免了无谓的强制类型转换。

泛型在集合中的应用：
```
ArrayList<String> list = new ArrayList<String>();	//true

ArrayList<Object> list = new ArrayList<String>();	//false
ArrayList<String> list = new ArrayList<Object>();	//false
```

考虑到新老系统兼用性：
```
ArrayList list = new ArrayList<String>();	//true
ArrayList<String> list = new ArrayList();	//true
```

注意：在泛型中没有多态的概念，两边的数据类型必须一致，或者只写一边的数据类型。


### java泛型总结2

需求：定义一个函数可以接收任意类型的参数，要求函数的返回值类型与实参的数据类型要一致。

答案代码：

```
public static <T> T print(T o) {
    return o;
}
```

自定义泛型：自定义泛型可以理解为是一个数据类型的占位符，或者理解为是一个数据类型的变量。

泛型方法：

泛型方法的定义格式：
```
修饰符 <声明自定义泛型> 返回值类型 函数名(形参列表..) {}
```

注意：

1. 在方法上的自定义泛型的具体数据类型是调用该方法的时候传入实参的时候确定的。
2. 自定义泛型使用的标识符只要符合标识符的命名规则即可。


### java泛型总结3

泛型类：自定义或java包内已定义的（例如ArrayList就是一个泛型类，还有Container）

泛型类的定义格式：

```
class 类名<声明自定义的泛型> {}
```

注意：

1. 在类上自定义的泛型的具体数据类型是在创建对象的时候指定的。

2. 在类上自定义了泛型，如果创建该类的对象时没有指定泛型的具体类型，那么默认是Object类型。


### java泛型总结4

泛型接口：自定义或java包内已定义的（例如生成器Generator，比较Comparator）

泛型接口的定义格式：

```
interface 接口名<声明自定义的泛型> {}
```

注意：

1. 在接口上自定义泛型的具体数据类型是在实现该接口的时候指定的。

2. 如果一个接口自定义了泛型，在实现该接口的时候没有指定具体的数据类型，那么默认是Object数据类型。

如果想在创建接口实现类对象的时候再指定接口自定义泛型 的具体类型：

```
class B<T> implements bb<T> {}
```


### java io方面

IO解决问题：解决设备与设备之间的数据传输问题。比如：硬盘--->内存     内存--->硬盘

**字节流**之输入字节流：

-----------------------| InputStream 所有输入字节流的基类，它是抽象类。

--------------------------| FileInputStream 读取文件的输入字节流。

--------------------------| BufferedInputStream 缓冲输入字节流。该类内部其实就是维护了一个8kb字节数组而已。该类出现的目的是为了提高读取文件数据的效率。

**字节流**之输出字节流：

-----------------------| OutputStream 所有输出字节流的基类，它是抽象类。

--------------------------| FileInputStream 读取文件的输出字节流

--------------------------| BufferedOutputStream 缓冲输出字节流。该类内部其实就是维护了一个8kb字节数组而已。该类出现的目的是为了提高向文件写数据的效率。

什么情况下使用字节流：读取到数据不需要经过编码或者解码的情况下这时候使用字节流。比如：图片数据

字符流 = 字节流 + 编码（解码）

**字符流**之输入字符流：

-----------------------| Reader 所有输入字符流的基类，它是抽象类。

--------------------------| FileReader 读取文件字符的输入字符流

--------------------------| BufferedReader 缓冲输入字符流。该类内部其实就是维护了一个8192个长度的字符数组而已。该类出现的目的是为了提高读取文件字符的效率并且拓展了功能（*readLine()*）。

**字符流**之输出字符流：

-----------------------| Writer 所有输出字符流的基类，它是抽象类。

--------------------------| FileWriter 向文件输出字符数据的输出字符流

--------------------------| BufferedWriter 缓冲输出字符流。该类内部其实就是维护了一个8192个长度的字符数组而已。该类出现的目的是为了提高写文件字符的效率并且拓展了功能（*newLine()*）。

什么情况下使用字符流：如果读写的都是字符数据，这时候我们就使用字符流。

**转换流**之输入字节流的转换流：InputStreamReader

**转换流**之输出字节流的转换流：OutputStreamWriter

转换流的作用：

(1) 可以把对应的字节流转换成字符流使用。

(2) 可以指定码表来读写文件的数据。



### java多线程

多线程的好处：多线程解决了在一个进程中同时可以执行多个任务代码的问题。

自定义线程的创建方式：

(1) 方式一：继承Thread.

>1.自定义一个类继承Thread类。

>2.重写Thread的run方法，把自定义线程的任务代码定义在run方法上。

>3.创建Thread子类的对象，并且调用start方法启动该线程。

代码：

```
public class demo extends Thread {
    @override
    public void run() {
        
    }
}
```

(2) 方式二：实现Runnable接口.

>1.自定义一个类实现Runnable接口。

>2.实现Runnable接口中的run方法，把自定义线程的任务代码定义在run方法上。

>3.创建Runnable实现类的对象。

>4.创建Thread对象，并且把Runnable实现类的对象作为参数传递。

>5.调用Thread对象的start方法启动该线程。

线程安全问题出现的根本原因：

>1.必须要存在两个或者两个以上的线程共享着一个资源。

>2.操作共享资源的代码必须有两句或者两句以上。

线程安全问题的解决方案：

>1.同步代码块

```
synchronized (锁) {
    //需要被同步的代码
}
```

>2.同步函数  

```
修饰符 synchronized 返回值类型 函数名(形参列表..) {
    //代码
}
```

注意：

>1.同步代码块的锁可以是任意的对象。同步函数的锁是固定的，非静态函数的锁对象是this对象，静态函数的锁对象是class对象。

>2.锁对象必须是多线程共享的对象，否则锁不住。

>3.在同步代码块或者是同步函数中调用sleep方法是不会释放锁对象的，如果是调用了wait方法是会释放锁对象的。


### junit单元测试

调试代码目前存在的问题：

>1.目前的方法如果需要测试，都需要在main()方法上调用。

>2.目前的结果都需要我们人工对比。

junit要注意的细节：

>1.如果使用junit测试一个方法的时候，在junit窗口上显示绿条那么代表测试正确，如果是出现了红条，则代表该方法测试出现了异常不通过。

>2.如果点击方法名、类名、包名、工程名运行junit分别测试的是对应的方法，类、包中的所有类的test方法，工程中的所有test方法。

>3.@Test测试的方法不能是static修饰也不能带有形参。

>4.如果测试一个方法的时候需要准备测试的环境或者是清理测试的环境，那么可以使用*@Before*、*@After*、*@BeforeClass*、*@AfterClass*这四种注解。*@Before*、*@After*是在每个测试方法测试的时候都会调用一次，*@BeforeClass*、*@AfterClass*是在所有的测试方法测试之前与测试之后调用一次而已。

junit使用规范：

>1.一个类如果需要测试，那么该类就应该对应着一个测试类，测试类的命名规范：*被测试的类的类名 + Test*。

>2.一个被测试的方法一般对应着一个测试的方法，测试的方法的命名规范是：*test + 被测试的方法的方法名*。


### java的Stack

若栈为`Stack<TreeNode>`，则空节点（TreeNode定义的）可以入栈。


### Eclipse自定义启动图标、任务栏图标、其它图标

环境：win10，绿色版Eclipse（linux下或者Eclipse的其它版本原理一样）

操作：

>1.启动图标目录为：*eclipse\plugins\org.eclipse.platform_4.4.2.v20150204-1700*，文名为：*splash.bmp*，只需替换该bmp图片即可（注意替换的bmp与被替换的bmp参数大小一致，其它格式转为bmp格式要使用图片软件的**另存为**，直接改后缀不可以。）

>2.任务栏图标目录为：*eclipse\plugins\org.eclipse.epp.package.jee_4.4.2.20150219-0708*，文件名为：*javaee-ide_x32.png*，只需替换该png图片即可（注意替换的png与被替换的png参数大小一致，其它格式转为png格式要使用图片软件的**另存为**，直接改后缀不可以。）

>3.其它各图标替换操作与上面步骤类似，找目录时可能会花费一些时间。


### eclipse更改字体使中英文大小协调

eclipse->window->preference->color and font->Basic->Text Font->Edit->Courier New字体


### windows下bat批处理文件

常用命令：

`pause`: 让当前控制台停留。

`echo sth`: 向控制台输出指定的内容（*sth*）。

`echo off`: 隐藏*echo off*后面执行的所有命令。

`@ `: 隐藏当前行执行的命令。

`title sth`: 把当前控制台窗口的标题改为*sth*。

`color XX`: 改变当前控制台的背景颜色与前景颜色（文字），两个*X*的取值范围是一位16进制数字。

`%sth%`: 代码注释，两个*%*之间为要注释的信息，且只能单行注释，即两个*%*必须在一行上。

`%X`: bat文件内部预留参数（变量），*X*取值范围是[1, 9]，最多有9个，需要传参且只能在控制台调用bat文件时传参。

### 控制台下运行java文件

可以把对应的*class*文件打包为*.jar*文件或者压缩为*.zip*文件，运行时指定目录位置就可以运行，例如：

`java -classpath .\big_integer.zip big_integer`


### javac，使用"-d ."与省略-d的区别

在当前工作目录下生成class文件，一般情况下有两种方法

>方法一： javac <srcFile>

>方法二： javac -d . <srcFile>

javac 的 -d参数用于指定生成class文件的位置，.(点号)表示当前目录。

所以两种方法相似，但不完全等同。如下例

例子：

当前目录是d:\temp，d:\temp下有个中类hello.java如下

```
package org.Hello;
public class hello{
    static public void main(String[] args){
        System.out.println("hello world");
    }
}
```

按照方法一，运行 `javac hello.java`，生成*hello.class*文件在*d:\temp*目录下。

按照方法二，运行 `javac -d . hello.java`，生成*hello.class*文件并不在*d:\temp*目录，而是在*d:\temp\org\Hello*。

删除`package org.Hello;`后，再分别用`javac hello.java`和`javac -d . hello.java`两种方法，生成的*hello.class*文件都是*d:\temp*目录

现在可以看出，-d参数的作用是指定生成java包的根目录，”-d .“应该这样理解更准确：在当前目录上编译生成java包。

如果省略了-d，则仅仅是在当前目录生成的class文件。

大多数情况都是编译生成java，尽量使用-d参数


### java对象的克隆

对象的浅克隆：

对象浅克隆要注意的细节：

>1.如果一个对象要调用clone()方法克隆，那么该对象所属的类要实现Cloneable接口。

>2.Cloneable接口只不过是一个标识接口而已，没有任何方法。

>3.对象的浅克隆就是克隆一个对象的时候，如果被克隆的对象中维护了另外一个类的对象，这时候只是克隆另外一个对象的地址，而没有把另外一个对象也克隆一份。

对象的深克隆：对象的深克隆就是利用对象的输入输出流把对象先写到文件上，然后在读取对象的信息，这个过程就称作为对象的深克隆。


### Arrays.sort()

1.Arrays.sort()中参数只有数组

```
int[] array = {5, 6, -1, 4}; 
Arrays.sort(array); 
```

这种是默认的排序，按照字典序(ASCII)的顺序进行排序。

2.Arrays.sort()中参数有数组和排序方法

>使用提供的方法 

```
String[] str = {“abc”, “aaa”, “abc”};
//String中定义的忽略大小写，完全通过字母的顺序进行排序 
Arrays.sort(str, String.CASE_INSENSITIVE_ORDER);
//反向排序
Arrays.sort(str, Collections.reverseOrder()); 
```

>自定义排序方法 

除了使用java提供的排序方法外，还可以使用自定义的排序方法，自定义排序方法需要实现java.util.Comparetor接口中的compare方法。

``` 
int compare(Object obj1, Object obj2) 
compare方法返回负数时代表obj1 < obj2 
compare方法返回0时代表obj1 < obj2 
compare方法返回正数时代表obj1 < obj2 
```


### java Sting 如何替换指定位置的字符

```
public static void main(String[] args) {
        String str = "****";
        if(StringUtils.isNotBlank(str)){
             StringBuilder sb = new StringBuilder("18698587234");
             sb.replace(3, 7, str);
             System.err.println("========"+sb.toString());
        }
    }
```


### java基本数据类型相互转换

String转char[]:

```
String str = "aaa";
char[] ch = str.toCharArray();
```

char[]转String:

```
char[] ch = "aaa";
String str = String.valueOf(ch);
```

String转char:

```
int i = 0;
char c = str.charAt(i);
```

char转String:

```
char c = 'a';
String str = String.valueOf(c);
```

String转int:

```
int i = Integer.parseInt(str); //注意int最多存放10位数
//字串转成 Double, Float, Long 的方法大同小异.
```

int转String:

```
int i =1234;
String str;
str = String.valueOf(i);
```
int转二进制String:

```
String ss = Integer.toBinaryString(8);
//ss="1000"
```


### java Object.finalize()方法

垃圾回收器回收对象的时候，首先要调用这个类的finalize()方法

java的finalize()方法大致相当于c++的析构函数，大部分时候我们使用Object类中的finalize()默认方法，当与其它语言混合使用时需要编写对应的finalize()方法释放资源。

任何一个对象的finalize()方法都只会被系统自动调用一次。


### jvm 判定对象是否存活

java对象回收机制没有使用引用计数算法，因为引用计数算法很难解决对象之间循环引用的问题。

所以java对象回收机制使用的是可达性分析算法。

如果对象在进行可达性分析后发现没有与*GC Roots*相连接的引用链，那它将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行finalize()方法。当对象没有覆盖finalize()方法，或者finalize()方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行”。

注意：每个对象在回收前都会调用这个类的finalize()方法，上面所说的是否有必要执行finalize()方法对应的问题是是否需要进行第二次标记

如果这个对象被判定为有必要执行finalize()方法，那么这个对象将会被放置在一个叫F-Queue的队列中，并在稍后由一个虚拟机自动建立的、低优先级的Finalizer线程去执行它。这里所谓的“执行”是指虚拟机会触发这个方法，但并不承诺会等待它运行结束（防止因为一个队列中的对象的finalize()方法运行慢或者死循环而使其它队列中的对象永久的处于等待）。

finalize()方法是对象逃脱死亡命运的最后一次机会，稍后GC将对F-Queue中的对象进行第二次小规模的标记，如果对象要在finalize()中成功拯救自己--只要重新与引用链上的任何一个对象建立关联即可，譬如把自己（*this*关键字）赋值给某个类变量或者对象的成员变量、类的静态变量，那么在第二次标记时它将被移除出“即将回收”的集合；如果对象这时候还没有逃脱，那基本上它就真的被回收了。


### java的hashCodeheequals的约定关系

1.如果两个对象相等，那么他们一定有相同的哈希值（hash code）。

2.如果这两个对象的哈希值相等，那么这两个对象有可能相等也有可能不想等。（需要再通过equals来判断）


### java的反射与内省

反射：当一个字节码文件加载到内存的时候，jvm会对该字节码进行解剖，然后会创建一个对象的Class对象，把字节码文件的信息全部都存储到该Class对象中，我们只要获取到Class对象，我们就可以使用字节码对象设置对象的属性或者调用对象的方法等操作。。。。

注意：在反射技术中一个类的任何成员都有对应的类进行描述。比如：成员变量（field）  方法--->Method类

内省：是一个变态的反射


### Class.forName()

参数为：包名+类名，根据参数获取到类并返回该类


### Apache BeanUtils

BeanUtils主要解决的问题：把对象的属性数据封装到对象中

BeanUtils的好处：

>1.BeanUtils设置属性值的时候，如果属性是基本数据类型，BeanUtils会自动帮我们转换数据类型。

>2.BeanUtils设置属性值的时候，底层也是依赖于get或者set方法设置以及获取属性值的。

>3.BeanUtils设置属性值，如果设置的属性是其他的引用类型数据，那么这时候必须要注册一个类型转换器。

BeanUtils使用的步骤：

>1.导包commons-logging.jar、commons-beanutils-1.8.0.jar


### java static{}块

静态代码块，在类的构造方法之前执行，并且只会在第一次执行，之后都不会执行的方法代码块。

像一段写在静态方法中的代码一样，只是，static中的代码在类加载时就会被执行，而静态方法是需要显示调用，并可以重复再次调用。

应用的场合：比如说比较耗资源的数据库连接啊之类的，在构造方法前调用，或者持有文件句柄之类的，都可以在静态代码库内使用，总之理解它的调用次序就可以啦。


### java配置文件、绝对路径与相对路径

配置文件：如果是经常发生变化的数据我们可以定义在配置文件上。比如说：数据库的用户名与密码。

绝对路径：一个文件的完整路径信息。缺点：系统间不通用

相对路径：相对路径是相对于当前程序的路径。当前路径就是执行java命令的时候，控制台所在的路径。

类文件路径：类文件路径就是使用了classpath的找对应的资源文件。如果需要使用类文件路径首先先要获取到一个Class对象。


### xml基础

1.xml的作用 :

>作为软件配置文件

>作为小型的数据库

2.xml语法（由w3c组织规定的）：

标签：标签名不能以数字开头，中间不能有空格，区分大小写，有且仅有一个根标签

属性：可以有多个属性，但属性值必须用引号（单引号或双引号）包含，但不能省略，也不能单双混用。

文档声明：`<?xml version="1.0" encoding="utf-8"?>`，（encoding="utf-8"：打开或解析xml文档时的编码，要和保存xml文档时的编码保持一致）

3.xml解析（程序读取或操作xml文档）:

两种解析方式：DOM解析与SAX解析

DOM解析原理：一次性的把xml文档加载成Document树，通过Document对象得到节点对象，通过节点对象访问xml文档内容（标签，属性，文本，注释）

#### Dom4j工具（基于DOM解析原理）：

1.读取xml文档

```
Document document = new SAXReader().read("xml文件路径"); //读取xml文档
Element rootElement = document.getRootElement(); //得到根节点
rootElement.nodeIterator(); //得到根基点的所有子节点（不包括孙节点）
rootElement.element("name"); //指定名称的第一个子标签对象
rootElement.elementIterator("name"); //指定名称的所有子标签对象
rootElement.elements(); //得到存有所有子标签对象的List
rootElement.attributeValue("name"); //指定名称的属性值
Attribute attr = rootElement.attribute("name"); //指定名称的属性对象
attr.getName(); //属性名称
attr.getValue(); //属性值
rootElement.attributeIterator(); //得到所有属性对象的迭代器
rootElement.attributes(); //得到所有属性对象的List
rootElement.getText(); //得到当前标签的文本
rootElement.elementText("name"); //得到指定名称的子标签的文本
```

2.修改xml文档

```
//增加
Document document = DocumentHelper.createDocument(); //增加文档
Element rootElement = document.addElement("名称"); //增加根标签
rootElement.addElement("名称"); //增加标签
rootElement.addAttribute("名称", "值"); //增加属性
//修改
Attribute.setValue("值"); //修改属性值
Element.addAttribute("同名的属性名", "属性值"); //修改同名的属性值
Element.setText("内容"); //修改文本内容
//删除
Element.detach(); //删除标签
Attribute.detach(); //删除属性
```

### XPath技术

XPath是一门在xml文档中查找信息的语言，XPath可用来在xml文档中对元素和属性进行遍历。

引入：当使用dom4j查询比较深的层次结构的节点（标签，属性，文本），比较麻烦

作用：主要是用于快速获取所需的节点对象

使用：

1.导入XPath支持jar包。 jaxen-1.1-beta-6.jar

2.使用XPath方法：

```
List<Node> selectNodes("XPath表达式"); //查询多个节点对象
Node selectSingleNode("XPath表达式"); //查询一个节点对象
```

3.XPath语法

```
类似文件系统的绝对路径与相对路径
/: 如果路径以单斜线/开头，那么该路径就表示到一个元素的绝对路径。
//: 如果路径以双斜线//开头，则表示选择文档中所有满足双斜线//之后规则的元素（无论层级关系）
*: 星号*表示选择所有由星号之前的路径所定位的元素
//*: 选择所有元素
[]: 方括号里的表达式可以进一步的指定元素，其中数字表示元素在选择集里的位置，而last()函数则表示选择集中的最后一个元素。
/AAA/BBB[1]: 选择AAA的第一个BBB子元素
/AAA/BBB[last()]: 选择AAA的最后一个BBB子元素
@: 属性通过前缀@来指定
//@id: 选择所有的id属性
//BBB[@id]: 选择有id属性的BBB元素
//BBB[@*]: 选择有任意属性的BBB元素
//BBB[not(@*)]: 选择没有属性的BBB元素
//BBB[@id='001']: 选择含有属性id且其值为'001'的BBB元素
and: 逻辑运算符，相当于&&
or: 逻辑运算符，相当于||
//BBB[@id='001' and @name='spcckai']: 选择id属性值为'001'且name属性值为spcckai的BBB元素
//BBB[@id='002' or @name='spcckai']: 选择id属性值为'002'或name属性值为spcckai的BBB元素
text(): 表示选择文本内容
//name/text(): 选择name标签下的子文本
```


### xml约束

xml约束要求：大家能够看懂约束内容，根据约束内容写出符合规则的xml文件。

引入：

>xml语法：规范的xml文件的基本编写规则。（由w3c组织制定的）

>xml约束：规范xml文件*数据内容格式*的编写规则。（由开发者自行定义）

xml约束技术：

>DTD约束：语法相对简单，功能也相对简单。学习成本也低

>Schema约束：语法相对复杂，功能也相对强大。学习成本相对高！！！


### DTD约束

1.导入dtd方式

内部导入：<!DOCTYPE 根元素 [元素声明]>

外部导入：

>本地文件系统：<!DOCTYPE 根元素 SYSTEM "文件名">

>公共的外部导入：<!DOCTYPE 根元素 PUBLIC "http://gz.itcast.cn/itcast.dtd"> 可以调用写好的框架

2.DTD语法：

约束标签：<!ELEMENT 元素名称 类别> 或 <!ELEMENT 元素名称 (元素内容)>

类别：

>空标签：EMPTY <!ELEMENT 元素名称 EMPTY>, 表示元素一定是空元素。

>普通字符串：#PCDATA。表示元素的内容一定是普通字符串（不能含有子标签）

>任何内容：ANY。表示元素的内容可以是任意内容（包括子标签）

元素内容：

>顺序问题：<!ELEMENT 元素名称 (子元素名称 1,子元素名称 2,.....)>：按顺序出现子标签

>次数问题：标签：必须只能出现1次   标签+：至少出现1次    标签*：出现0次或n次    标签?：出现0次或1次

约束属性：

整体结构：<!ATTLIST 元素名称 属性名称 属性类型 默认值>

默认值：

>"xxxx" 属性未赋值的默认值

>#REQUIRED 属性值是必需的 

>#IMPLIED 属性不是必需的 

>#FIXED value 属性值是固定的 (value是具体数据)

属性类型(控制属性值)：

CDATA: 表示普通字符串

(en1|en2|..): 表示一定是任选其中的一个值

ID: 表示在一个xml文档中该属性值必须唯一。值不能以数字开头


### Schema约束

名称空间：告诉 xml文档的 元素被哪个schema文档约束。


### 软件两大结构

>C/S: Client-Server，有客户端程序

>B/S: Broswer-Server，无客户端程序，只需浏览器


### tomcat基本使用

#### 1.下载并按照
			
1）到apache官网。www.apache.org     http://jakarta.apache.org(产品的主页)

2）

>安装版：window （exe、msi） linux（rmp）

>压缩版：window（rar，zip） linux（tar，tar.gz）学习时候使用
			
3）运行和关闭tomcat
					
3).1 启动软件

a）找到%tomcat%/bin/startup.bat ，双击这个文件

b）弹出窗口，显示信息（不要关闭次窗口）

c）打开浏览器，输出以下地址 http://localhost:8080

d）看到一只猫画面，证明软件启动成功！

3).2 关闭软件

a）找到%tomcat%/bin/shutdown.bat，双击这个文件即可！

c）打开浏览器，输出以下地址。看到“无法连接”（最好先清空浏览器缓存）

#### 2.tomcat软件使用的常见问题

1）闪退问题

原因：tomcat软件是java语言开发的。 tomcat软件启动时，会默认到系统的环境变量中查找一个名称叫JAVA_HOME的变量。这个变量的作用找到tomcat启动所需的jvm。

解决办法； 到环境变量中设置JAVA_HOME的变量

JAVA_HOME= C:\Program Files\Java\jdk1.6.0_30  (注意别配置到bin目录下)
						
2）端口占用的错误

原因： tomcat启动所需的端口被其他软件占用了！

解决办法： 

a）关闭其他软件程序，释放所需端口

b）修改tomcat软件所需端口

找到并修改%tomcat%/conf/server.xml文件
						
```
<Connector port="8081" protocol="HTTP/1.1" 
               connectionTimeout="20000" 
               redirectPort="8443" />
```

3）CATALINA环境变量问题

原因： tomcat软件启动后，除了查找JAVA_HOME后，还会再查找一个叫CATALINA_HOME变量，这个变量的作用是设置tomcat的根目录。

解决办法：建议不要设置CATALINA_HOME变量。检查如果有的话，清除掉！！！

#### 3.体验tomcat软件作用

webapps目录： tomcat共享目录。需要共享的本地资源放到此目录中。
	
#### 4.URL

URL全名叫统一资源定位符，用于定位互联网的资源。
		
问题： http://localhost:8081/myweb/test.html  看到文件？

http://     协议。http协议。

localhost    域名。为了找到IP地址。

本地域名： localhost

外部域名：www.baidu.com

8081       端口。软件监听的

8080： tomcat默认的端口

3306：mysql数据库的端口

1521： orace数据库的端口。

/myweb:   web应用的名称。默认情况下，在webapps目录下找

/test.html  ： 资源名称。
		
#### 5.Tomcat的目录结构

|-bin: 存放tomcat的命令。

catalina.bat 命令：

startup.bat  -> catalina.bat start	

shutdown.bat - > catalina.bat stop

|- conf: 存放tomcat的配置信息。其中server.xml文件是核心的配置文件。

|-lib：支持tomcat软件运行的jar包。其中还有技术支持包，如servlet，jsp

|-logs：运行过程的日志信息

|-temp: 临时目录

|-webapps： 共享资源目录。web应用目录。（注意不能以单独的文件进行共享）

|-work： tomcat的运行目录。jsp运行时产生的临时文件就存放在这里


### web应用的目录结构

```
|- WebRoot/ :   web应用的根目录
		|- 静态资源（html+css+js+image+vedio）
		|- WEB-INF/ ： 固定写法。
			|-classes/： （可选）固定写法。存放class字节码文件
			|-lib/： （可选）固定写法。存放jar包文件。
			|-web.xml    
```
	
注意：

>1）WEB-INF目录里面的资源不能通过浏览器直接访问

>2）如果希望访问到WEB-INF里面的资源，就必须把资源配置到一个叫web.xml的文件中。


### servlet技术

Servlet技术： 用java语言开发动态资源的技术
				
开发一个Servlet程序的步骤：
						
1）创建一个java类，继承HttpServlet类

2）重写HttpServlet类的doGet方法

3）把写好的servlet程序交给tomcat服务器运行！！！！

3.1 把编译好的servlet的class文件拷贝到tomcat的一个web应用中。（web应用										的WEB-INF/classes目录下）		

3.2 在当前web应用的web.xml文件中配置servlet

```
<!-- servlet配置 -->
<servlet>
    <servlet-name>HelloServlet</servlet-name>
    <servlet-class>gz.itcast.HelloServlet</servlet-class>
</servlet>
<!--  servlet的映射配置 -->
<servlet-mapping>
    <servlet-name> HelloServlet </servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

4）访问servlet

http://localhost:8080/myweb/hello


### http协议

http协议： 对浏览器客户端 和  服务器端 之间数据传输的格式规范


### 查看http协议的工具

1）使用火狐的firebug插件（右键->firebug->网络）

2）使用谷歌的“审查元素”

3）使用系统自带的telnet工具（远程访问工具）				

>a）telnet localhost 8080      访问tomcat服务器

>b）ctrl+]     回车          可以看到回显

>c）输入请求内容

```							
GET /day09/hello HTTP/1.1
Host: localhost:8080
```

>d）回车，即可查看到服务器响应信息。


### http协议内容

请求（浏览器->服务器）

```
GET /day09/hello HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: keep-alive
```

响应（服务器->浏览器）

```
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Length: 24
Date: Fri, 30 Jan 2015 01:54:57 GMT

this is hello servlet!!!
```


### Http请求

```
GET /day09/hello HTTP/1.1               -请求行
Host: localhost:8080                    --请求头（多个key-value对象）
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: keep-alive
                                    --一个空行
name=eric&password=123456             --（可选）实体内容
```

1. 请求行

GET /day09/hello HTTP/1.1 

1.1 http协议版本

http1.0：当前浏览器客户端与服务器端建立连接之后，只能发送一次请求，一次请求之后连接关闭。

http1.1：当前浏览器客户端与服务器端建立连接之后，可以在一次连接中发送多次请求。（基本都使用1.1）

1.2 请求资源

URL:  统一资源定位符。http://localhost:8080/day09/testImg.html。只能定位互联网资源。是URI的子集。

URI： 统一资源标记符。/day09/hello。用于标记任何资源。可以是本地文件系统，局域网的资源（//192.168.14.10/myweb/index.html），可以是互联网。

1.3 请求方式

常见的请求方式： GET 、 POST、 HEAD、 TRACE、 PUT、 CONNECT 、DELETE	

常用的请求方式： GET  和 POST	

表单提交：

```
<form action="提交地址" method="GET/POST">	

<form>
```

GET   vs  POST 区别

1.3.1 GET方式提交 

a）地址栏（URI）会跟上参数数据。以？开头，多个参数之间以&分割。

```
GET /day09/testMethod.html?name=eric&password=123456 HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://localhost:8080/day09/testMethod.html
Connection: keep-alive
```

b）GET提交参数数据有限制，不超过1KB。

c）GET方式不适合提交敏感密码。

d）注意： 浏览器直接访问的请求，默认提交方式是GET方式

1.3.2 POST方式提交

a）参数不会跟着URI后面。参数而是跟在请求的实体内容中。没有？开头，多个参数之间以&分割。

```
POST /day09/testMethod.html HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://localhost:8080/day09/testMethod.html
Connection: keep-alive

name=eric&password=123456
```

b）POST提交的参数数据没有限制。

c）POST方式提交敏感数据。

2. 请求头

```
Accept: text/html,image/*      -- 浏览器接受的数据类型
Accept-Charset: ISO-8859-1     -- 浏览器接受的编码格式
Accept-Encoding: gzip,compress  --浏览器接受的数据压缩格式
Accept-Language: en-us,zh-       --浏览器接受的语言
Host: www.it315.org:80          --（必须的）当前请求访问的目标地址（主机:端口）
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT  --浏览器最后的缓存时间
Referer: http://www.it315.org/index.jsp      -- 当前请求来自于哪里
User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)  --浏览器类型
Cookie:name=eric                     -- 浏览器保存的cookie信息
Connection: close/Keep-Alive            -- 浏览器跟服务器连接状态。close: 连接关闭  keep-alive：保存连接。
Date: Tue, 11 Jul 2000 18:23:51 GMT      -- 请求发出的时间
```

3. 实体内容

只有POST提交的参数会放到实体内容中


### Unicode、UTF－8 和 ISO8859-1到底有什么区别

说明：本文转载于新浪博客，旨在方便知识总结。原文地址：http://blog.sina.com.cn/s/blog_673c81990100t1lc.html

本文主要包括以下几个方面：编码基本知识，java，系统软件，url，工具软件等。

在下面的描述中，将以"中文"两个字为例，经查表可以知道其GB2312编码是"d6d0 cec4"，Unicode编码为"4e2d 6587"，UTF编码就是"e4b8ad e69687"。注意，这两个字没有iso8859-1编码，但可以用iso8859-1编码来"表示"。

1. 编码基本知识

最早的编码是iso8859-1，和ascii编码相似。但为了方便表示各种各样的语言，逐渐出现了很多标准编码，重要的有如下几个。

1.1. iso8859-1

属于单字节编码，最多能表示的字符范围是0-255，应用于英文系列。比如，字母a的编码为0x61=97。

很明显，iso8859-1编码表示的字符范围很窄，无法表示中文字符。但是，由于是单字节编码，和计算机最基础的表示单位一致，所以很多时候，仍旧使用iso8859-1编码来表示。而且在很多协议上，默认使用该编码。比如，虽然"中文"两个字不存在iso8859-1编码，以gb2312编码为例，应该是"d6d0 cec4"两个字符，使用iso8859-1编码的时候则将它拆开为4个字节来表示："d6 d0 ce c4"（事实上，在进行存储的时候，也是以字节为单位处理的）。而如果是UTF编码，则是6个字节"e4 b8 ad e6 96 87"。很明显，这种表示方法还需要以另一种编码为基础。

1.2. GB2312/GBK

这就是汉字的国标码，专门用来表示汉字，是双字节编码，而英文字母和iso8859-1一致（兼容iso8859-1编码）。其中gbk编码能够用来同时表示繁体字和简体字，而gb2312只能表示简体字，gbk是兼容gb2312编码的。

1.3. unicode

这是最统一的编码，可以用来表示所有语言的字符，而且是定长双字节（也有四字节的）编码，包括英文字母在内。所以可以说它是不兼容iso8859-1编码的，也不兼容任何编码。不过，相对于iso8859-1编码来说，uniocode编码只是在前面增加了一个0字节，比如字母a为"00 61"。

需要说明的是，定长编码便于计算机处理（注意GB2312/GBK不是定长编码），而unicode又可以用来表示所有字符，所以在很多软件内部是使用unicode编码来处理的，比如java。

1.4. UTF

考虑到unicode编码不兼容iso8859-1编码，而且容易占用更多的空间：因为对于英文字母，unicode也需要两个字节来表示。所以unicode不便于传输和存储。因此而产生了utf编码，utf编码兼容iso8859-1编码，同时也可以用来表示所有语言的字符，不过，utf编码是不定长编码，每一个字符的长度从1-6个字节不等。另外，utf编码自带简单的校验功能。一般来讲，英文字母都是用一个字节表示，而汉字使用三个字节。

注意，虽然说utf是为了使用更少的空间而使用的，但那只是相对于unicode编码来说，如果已经知道是汉字，则使用GB2312/GBK无疑是最节省的。不过另一方面，值得说明的是，虽然utf编码对汉字使用3个字节，但即使对于汉字网页，utf编码也会比unicode编码节省，因为网页中包含了很多的英文字符。

2. java对字符的处理

在java应用软件中，会有多处涉及到字符集编码，有些地方需要进行正确的设置，有些地方需要进行一定程度的处理。

2.1. getBytes(charset)

这是java字符串处理的一个标准函数，其作用是将字符串所表示的字符按照charset编码，并以字节方式表示。注意字符串在java内存中总是按unicode编码存储的。比如"中文"，正常情况下（即没有错误的时候）存储为"4e2d 6587"，如果charset为"gbk"，则被编码为"d6d0 cec4"，然后返回字节"d6 d0 ce c4"。如果charset为"utf8"则最后是"e4 b8 ad e6 96 87"。如果是"iso8859-1"，则由于无法编码，最后返回 "3f 3f"（两个问号）。

2.2. new String(Bytes, charset)

这是java字符串处理的另一个标准函数，和上一个函数的作用相反，将字节数组按照charset编码进行组合识别，最后转换为unicode存储。参考上述getBytes的例子，"gbk" 和"utf8"都可以得出正确的结果"4e2d 6587"，但iso8859-1最后变成了"003f 003f"（两个问号）。

因为utf8可以用来表示/编码所有字符，所以new String( str.getBytes( "utf8" ), "utf8" ) === str，即完全可逆。

2.3. setCharacterEncoding()

该函数用来设置http请求或者相应的编码。

对于request，是指提交内容的编码，指定后可以通过getParameter()则直接获得正确的字符串，如果不指定，则默认使用iso8859-1编码，需要进一步处理。参见下述"表单输入"。值得注意的是在执行setCharacterEncoding()之前，不能执行任何getParameter()。java doc上说明：This method must be called prior to reading request parameters or reading input using getReader()。而且，该指定只对POST方法有效，对GET方法无效。分析原因，应该是在执行第一个getParameter()的时候，java将会按照编码分析所有的提交内容，而后续的getParameter()不再进行分析，所以setCharacterEncoding()无效。而对于GET方法提交表单是，提交的内容在URL中，一开始就已经按照编码分析所有的提交内容，setCharacterEncoding()自然就无效。

对于response，则是指定输出内容的编码，同时，该设置会传递给浏览器，告诉浏览器输出内容所采用的编码。


### 如何开发一个servlet

步骤：

1. 编写java类，继承HttpServlet类；

2. 重写doGet和doPost方法；

3. Servlet程序交给tomcat服务器运行。

>3.1 servlet程序的class码拷贝到WEB-INF/classes目录；

>3.2 在web.xml文件中进行配置。

注意：使用Eclipse创建的是Dtnamic Web Project，初始程序创建后，Eclipse提供了一种替代web.xml中servlet的url命名问题，直接在java程序类上面一行添加：@WebServlet("/user_defined_url")，用了这个web.xml中相关配置就不会生效，具体用哪个自己决定。

问题：访问此url，http://localhost:8080/webTestDat10/first

前提：tomcat服务器启动时，首先加载webapps中的每一个web应用的web.xml配置文件；

http://: http协议

localhost: 到本地的host文件中查找是否存在该域名对应的IP地址；     ->127.0.0.1

8080: 找到tomcat服务器

/tebTestDay10: 在tomcat的webapps目录下找webTestDay10的目录

/first: 资源名称。

>1)在webTestDay10的web.xml中查找是否有匹配的url-pattern的内容（/first）；

>2)如果找到匹配的url-pattern，则使用当前servlet-name的名称到web.xml文件中查询是否有相同名称的servlet配置；

>3)如果找到，则取出对应的servlet配置信息中的servlet-class内容；最终要得到的就是这个字符串："FirstServlet.helloWorld"；

>4)通过反射：构造helloWorld对象，然后调用helloWorld里面的方法。


### servlet的映射路径

精确匹配：必须写完整的url，错一个就访问不到；eg: <url-pattern>/first</urlpattern>

模糊匹配：

><url-pattern>/*</urlpattern>: 浏览器输入http://localhost:8080/webTestDay10/任意路径

><url-pattern>/itcast/*</urlpattern>: 浏览器输入http://localhost:8080/webTestDay10/itcast/任意路径

><url-pattern>*.后缀名</urlpattern>: 浏览器输入http://localhost:8080/webTestDay10/任意路径.后缀名

><url-pattern>*.html</urlpattern>(伪静态): 浏览器输入http://localhost:8080/webTestDay10/任意路径.html

注意：

1. url-pattern要么以/开头，要么以*开头;

2. 不能同时使用两种模糊匹配，例如/itcast/*.do;

3. 当有输入的URL有多个servlet同时被匹配的情况下:

>3.1 精确匹配优先（长得最像优先被匹配）；

>3.2 以后缀名结尾的模糊匹配优先级最低！！


### servlet缺省路径

servlet的缺省路径是在tomcat服务器内置的一个路径。该路径对应的是一个DefaultServlet（缺省Servlet）。这个缺省Servlet的作用是解析web应用的静态资源文件。


####


