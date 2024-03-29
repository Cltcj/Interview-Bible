### 一、内联、宏定义、函数、typedef、static

#### 1、全局变量和static变量的区别

全局变量（外部变量）的说明之前再冠以static就构成了静态的全局变量。

全局变量本身就是静态存储方式，静态全局变量当然也是静态存储方式。<u>这两者在存储方式上并无不同</u>。

这两者的**区别**在于：

1. 非静态全局变量的作用域是整个源程序，当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。**而静态全局变量**则限制了其作用域，即只在定义该变量的源文件内有效，在同一源程序的其它源文件中不能使用它。由于静态全局变量的作用域限于一个源文件内，只能为该源文件内的函数公用，因此可以避免在其他源文件中引起错误。
2. **static全局变量只初始化一次，防止在其他文件单元被引用。**

#### 2. static函数与普通函数有什么区别？ 

static函数与普通的函数作用域不同。即在本文件中。只在当前源文件中使用的函数应该声明为内部函数（static），内部函数应该在当前源文件中声明和定义。

对于可在当前源文件以外使用的函数应该在一个头文件中说明，要使用这些函数的源文件要包含这个头文件。 **static函数与普通函数最主要区别是**<u>static函数在内存中只有一份</u>，普通静态函数在每个被调用中维持一份拷贝程序的局部变量存在于（堆栈）中，全局变量存在于（静态区）中，动态申请数据存在于（堆）

#### 3、静态成员与普通成员的区别是什么？

1.生命周期

静态成员变量从类被加载开始到类被卸载，一直存在；

普通成员变量只有在类创建对象后才开始存在，对象结束，它的生命期结束；

2.共享方式

静态成员变量是全类共享；普通成员变量是每个对象单独享用的；

3.定义位置

普通成员变量存储在栈或堆中，而静态成员变量存储在静态全局区；

4.初始化位置

普通成员变量在类中初始化；静态成员变量在类外初始化；

5.默认实参

可以使用静态成员变量作为默认实参，

#### 4、define、const、typedef、inline的使用方法？他们之间有什么区别？

**一、说说const和define的区别**

const用于定义常量；而define用于定义宏，而宏也可以用于定义常量。都用于常量定义时，它们的区别有：

1. const生效于编译的阶段；define生效于预处理阶段。
2. const定义的常量，在C语言中是存储在内存中、需要额外的内存空间的；define定义的常量，运行时是直接的操作数，并不会存放在内存中。
3. const定义的常量是带类型的；define定义的常量不带类型。因此define定义的常量不利于类型检查。

**二、#define和别名typedef的区别**

1. 执行时间不同，typedef在编译阶段有效，typedef有类型检查的功能；#define是宏定义，发生在预处理阶段，不进行类型检查；
2. 功能差异，typedef用来定义类型的别名，定义与平台无关的数据类型，与struct的结合使用等。#define不只是可以为类型取别名，还可以定义常量、变量、编译开关等。
3. 作用域不同，#define没有作用域的限制，只要是之前预定义过的宏，在以后的程序中都可以使用。而typedef有自己的作用域。

**三、 define与inline的区别**

1. \#define是关键字，inline是函数；
2. 宏定义在预处理阶段进行文本替换，inline函数在编译阶段进行替换；
3. inline函数有类型检查，相比宏定义比较安全；

#### 5、**static的用法和作用？**

**1.先来介绍它的第一条也是最重要的一条：隐藏。**（static函数，static变量均可），当同时编译多个文件时，所有未加static前缀的全局变量和函数都具有全局可见性。

**2.static的第二个作用是保持变量内容的持久。**（static变量中的记忆功能和全局生存期）存储在静态数据区的变量会在程序刚开始运行时就完成初始化，也是唯一的一次初始化。共有两种变量存储在静态存储区：全局变量和static变量，只不过和全局变量比起来，static可以控制变量的可见范围，说到底static还是用来隐藏的。

**3.static的第三个作用是默认初始化为0（static变量）**，其实全局变量也具备这一属性，因为全局变量也存储在静态数据区。在静态数据区，内存中所有的字节默认值都是0x00，某些时候这一特点可以减少程序员的工作量。

**4.static的第四个作用：C++中的类成员声明static**

① 函数体内static变量的作用范围为该函数体，不同于auto变量，该变量的内存只被分配一次，因此其值在下次调用时仍维持上次的值；

② 在模块内的static全局变量可以被模块内所有函数访问，但不能被模块外其它函数访问；

③ 在模块内的static函数只可被这一模块内的其它函数调用，这个函数的使用范围被限制在声明它的模块内；

④ 在类中的static成员变量属于整个类所拥有，对类的所有对象只有一份拷贝；

⑤ **在类中的static成员函数属于整个类所拥有，这个函数不接收this指针，因而只能访问类的static成员变量。**

类内：

⑥  **static类对象必须要在类外进行初始化**，static修饰的变量先于对象存在，所以static修饰的变量要在类外初始化；

⑦ 由于static修饰的类成员属于类，不属于对象，因此**static类成员函数是没有this指针的，this指针是指向本对象的指针。正因为没有this指针，所以static类成员函数不能访问非static的类成员，只能访问 static修饰的类成员**；

⑧ **static成员函数不能被virtual修饰，static成员不属于任何对象或实例，所以加上virtual没有任何实际意义**；静态成员函数没有this指针，虚函数的实现是为每一个对象分配一个vptr指针，而vptr是通过this指针调用的，所以不能为virtual；虚函数的调用关系，`this->vptr->ctable->virtual function`

#### 6、说说 static关键字的作用

1. **定义全局静态变量和局部静态变量**：在变量前面加上static关键字。初始化的静态变量会在数据段分配内存，未初始化的静态变量会在BSS段分配内存。直到程序结束，静态变量始终会维持前值。只不过全局静态变量和局部静态变量的作用域不一样；

2. **定义静态函数**：在函数返回类型前加上static关键字，函数即被定义为静态函数。静态函数只能在**本源文件**中使用；

3. 在变量类型前加上static关键字，变量即被定义为静态变量。**静态变量只能在本源文件中使用**；

   ```C++
   //示例 
   static int a; static void func();
   ```

4. 在c++中，**static关键字可以用于定义类中的静态成员变量**：使用静态数据成员，它既可以被当成全局变量那样去存储，但又被隐藏在类的内部。类中的static静态数据成员拥有一块单独的存储区，而不管创建了多少个该类的对象。所有这些对象的静态数据成员都**共享**这一块静态存储空间。

5. 在c++中，**static关键字可以用于定义类中的静态成员函数**：与静态成员变量类似，类里面同样可以定义静态成员函数。只需要在函数前加上关键字static即可。如静态成员函数也是类的一部分，而不是对象的一部分。所有这些对象的静态数据成员都**共享**这一块静态存储空间。

当调用一个对象的非静态成员函数时，系统会把该对象的起始地址赋给成员函数的this指针。而静态成员函数不属于任何一个对象，因此C++规定静态成员函数没有this指针（划重点，面试题常考）。既然它没有指向某一对象，也就无法对一个对象中的非静态成员进行访问。

#### 7、宏定义和函数有何区别？

① 宏在编译时完成替换，之后替换后的程序参与编译，所以宏就相当于直接插入代码，运行时不存在函数调用，执行起来更快，函数调用运行时需要跳到具体的调用函数。

② 宏定义无返回值，函数有返回值

③ 宏定义参数没有类型，不需要进行类型判断，函数参数具有类型，需要类型检查

④ 宏定义不需要在最后加分号。

#### 8、宏定义和typedef区别？

① 宏主要用于定义常量及书写复杂的内容，typedef主要用于定义类型的别名

② 宏替换发生在编译之前属于文本插入替换，typedef是编译的一部分

③ 宏不检查类型，typedef会检查类型

④ 宏不是语句不加分号，typedef是语句要加分号标识结束

⑤ 注意对指针的操作，typedef char* p_char和#define p_char char*差别巨大

#### 9、define宏定义和const的区别

**编译阶段**

define是在编译的预处理阶段起作用，而const是在编译、运行的时候起作用

**安全性**

define只做替换，不做类型检查和计算，也不求解，容易产生错误，一般最好加上一个大括号包含住全部的内容，要不然很容易出错

const常量有数据类型，编译器可以对其进行类型安全检查

**内存占用**

define只是将宏名称进行替换，在内存中会产生多分相同的备份。const在程序运行中只有一份备份，且可以执行常量折叠，能将复杂的的表达式计算出结果放入常量表

宏替换发生在编译阶段之前，属于文本插入替换；const作用发生于编译过程中。

宏不检查类型；const会检查数据类型。

宏定义的数据没有分配内存空间，只是插入替换掉；const定义的变量只是值不能改变，但要分配内存空间。

#### 10、C++中const和static的作用

**Static**

**不考虑类的情况**：

① 隐藏。所有不加static的全局变量和函数具有全局可见性，可以在其他文件中使用，加了之后只能在该文件所在的编译模块中使用

② 默认初始化为0

③ 静态变量在函数内定义，始终存在，且只进行一次初始化，具有记忆性，其作用范围与局部变量相同，函数退出后仍然存在，但不能使用

**考虑类的情况：**

① static成员变量：只与类关联，不与类的对象关联。定义时要分配空间，不能在类声明中初始化，必须在类外部初始化，初始化时不需要标示为static；可以被非static成员函数任意访问。

② static成员函数：不具有this指针，无法访问类对象的非static成员变量和非static成员函数；不能被声明为const、虚函数和volatile；可以被非static成员函数任意访问

**Const**

**不考虑类的情况：**

① const常量在定义时必须初始化，之后无法更改

② const形参可以接收const和非const类型的实参

考虑类的情况：

**①** **const成员变量：不能在类定义外部初始化，只能通过构造函数初始化列表进行初始化**

② const成员函数：const对象不可以调用非const成员函数；非const对象都可以调用；不可以改变非mutable数据的值

#### 11、C++的顶层const和底层const

**概念区分**

顶层const：指的是const修饰的变量本身是一个常量，无法修改，指的是指针，就是 * 号的右边

底层const：指的是const修饰的变量所指向的对象是一个常量，指的是所指变量，就是 * 号的左边

区分作用：

执行对象拷贝时有限制，常量的底层const不能赋值给非常量的底层const

使用命名的强制类型转换函数const_cast时，只能改变运算对象的底层const

#### 12、const关键字的作用有哪些?

1.阻止一个变量被改变，可以使用const关键字。在定义该const变量时，通常需要对它进行初始化，因为以后就没有机会再去改变它了；

2.对指针来说，可以指定指针本身为const，也可以指定指针所指的数据为const，或二者同时指定为const；

3.在一个函数声明中，const可以修饰形参，表明它是一个输入参数，在函数内部不能改变其值；

4.对于类的成员函数，若指定其为const类型，则表明其是一个常函数，不能修改类的成员变量，类的常对象只能访问类的常成员函数；

5.对于类的成员函数，有时候必须指定其返回值为const类型，以使得其返回值不为“左值”。

6.const成员函数可以访问非const对象的非const数据成员、const数据成员，也可以访问const对象内的所有数据成员；

7.非const成员函数可以访问非const对象的非const数据成员、const数据成员，但不可以访问const对象的任意数据成员；

8.一个没有明确声明为const的成员函数被看作是将要修改对象中数据成员的函数，而且编译器不允许它为一个const对象所调用。因此const对象只能调用const成员函数。

9.const类型变量可以通过类型转换符const_cast将const类型转换为非const类型；

10.const类型变量必须定义的时候进行初始化，因此也导致如果类的成员变量有const类型的变量，那么该变量必须在类的初始化列表中进行初始化；

11.对于函数值传递的情况，因为参数传递是通过复制实参创建一个临时变量传递进函数的，函数内只能改变临时变量，但无法改变实参。则这个时候无论加不加const对实参不会产生任何影响。但是在引用或指针传递函数调用中，因为传进去的是一个引用或指针，这样函数内部可以改变引用或指针所指向的变量，这时const 才是实实在在地保护了实参所指向的变量。因为在编译阶段编译器对调用函数的选择是根据实参进行的，所以**只有引用传递和指针传递可以用是否加const来重载**。一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开来。

#### 13、静态变量什么时候初始化

初始化只有一次，但是可以多次赋值，在主程序之前，编译器已经为其分配好了内存。

静态局部变量和全局变量一样，数据都存放在全局区域，所以在主程序之前，编译器已经为其分配好了内存但在C和C++中静态局部变量的初始化节点又有点不太一样。

**在C中，初始化发生在代码执行之前，编译阶段分配好内存之后，就会进行初始化\****，所以我们看到在C语言中无法使用变量对静态局部变量进行初始化，在程序运行结束，变量所处的全局内存会被全部回收。

而在**C++中，初始化时在执行相关代码时才会进行初始化，主要是由于C++引入对象后，要进行初始化必须执行相应构造函数和析构函数，在构造函数或析构函数中经常会需要进行某些程序中需要进行的特定操作，并非简单地分配内存。**所以C++标准定为全局或静态对象是有首次用到时才会进行构造，并通过atexit()来管理。在程序结束，按照构造顺序反方向进行逐个析构。所以在C++中是可以使用变量对静态局部变量进行初始化的。

#### 14、说说内联函数和宏定义的区别

区别：

1. **宏定义不是函数**，但是使用起来像函数。预处理器用复制宏代码的方式代替函数的调用，省去了函数压栈退栈过程，提高了效率；**而内联函数本质上是一个函数**，内联函数一般用于函数体的代码比较简单的函数，不能包含复杂的控制语句，while、switch，并且内联函数本身不能直接调用自身。
2. **宏函数**是在预编译的时候把所有的宏名用宏体来替换，简单的说就是字符串替换 ；**而内联函数**则是在编译的时候进行代码插入，编译器会在每处调用内联函数的地方直接把内联函数的内容展开，这样可以省去函数的调用的开销，提高效率
3. **宏定义**是没有类型检查的，无论对还是错都是直接替换；**而内联函数**在编译的时候会进行类型的检查，内联函数满足函数的性质，比如有返回值、参数列表等

**1、使用时的一些注意事项：**

- 使用宏定义一定要注意错误情况的出现，比如宏定义函数没有类型检查，可能传进来任意类型，从而带来错误，如举例。还有就是括号的使用，宏在定义时要小心处理宏参数，一般用括号括起来，否则容易出现二义性
- inline函数一般用于比较小的，频繁调用的函数，这样可以减少函数调用带来的开销。只需要在函数返回类型前加上关键字inline，即可将函数指定为inline函数。
- 同其它函数不同的是，最好将inline函数定义在头文件，而不仅仅是声明，因为编译器在处理inline函数时，需要在调用点内联展开该函数，所以仅需要函数声明是不够的。

**2、内联函数使用的条件：**

- 内联是以代码膨胀（复制）为代价，仅仅省去了函数调用的开销，从而提高函数的执行效率。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率 的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。以下情况不宜使用内联：
- （1）如果函数体内的代码比较长，使用内联将导致内存消耗代价较高。
- （2）如果函数体内出现循环，那么执行函数体内代码的时间要比函数调用的开销大。
- 内联不是什么时候都能展开的，一个好的编译器将会根据函数的定义体，自动地取消不符合要求的内联。

#### 15、说说内联函数和函数的区别，内联函数的作用。

1. 内联函数比普通函数多了关键字**inline**
2. 内联函数避免了函数调用的**开销**；普通函数有调用的开销
3. 普通函数在被调用的时候，需要**寻址（函数入口地址）**；内联函数不需要寻址。
4. 内联函数有一定的限制，内联函数体要求**代码简单**，不能包含复杂的结构控制语句；普通函数没有这个要求。

**内联函数的作用**：内联函数在调用时，是将调用表达式用内联函数体来替换。避免函数调用的开销。

在使用内联函数时，应注意如下几点：

1. 在内联函数内不允许用循环语句和开关语句。 如果内联函数有这些语句，则编译将该函数视同普通函数那样产生函数调用代码,递归函数是不能被用来做内联函数的。内联函数只适合于只有1～5行的小函数。对一个含有许多语句的大函数，函数调用和返回的开销相对来说微不足道，所以也没有必要用内联函数实现。
2. 内联函数的定义必须出现在内联函数第一次被调用之前。

#### 16、为什么不能把所有的函数写成内联函数?

内联函数以代码复杂为代价，它以省去函数调用的开销来提高执行效率。所以一方面如果内联函数体内代码执行时间相比函数调用开销较大，则没有太大的意义；另一方面每一处内联函数的调用都要复制代码，消耗更多的内存空间，因此以下情况不宜使用内联函数：

函数体内的代码比较长，将导致内存消耗代价

函数体内有循环，函数执行时间要比函数调用开销大

#### 17、说说include头文件的顺序以及双引号""和尖括号<>的区别

1. 区别：

   （1）尖括号<>的头文件是**系统文件**，双引号""的头文件是**自定义文件**。

   （2）编译器预处理阶段查找头文件的路径不一样。

2. 查找路径：

   （1）使用尖括号<>的头文件的查找路径：编译器设置的头文件路径-->系统变量。

   （2）使用双引号""的头文件的查找路径：当前头文件目录-->编译器设置的头文件路径-->系统变量。

### 二、C++特点及其它语言区别

#### 18、C++语言的特点

1. C++在C语言基础上引入了**面对对象**的机制，同时也**兼容C语言**。
2. C++有三大特性（1）封装。（2）继承。（3）多态；
3. C++语言编写出的程序结构清晰、易于扩充，程序**可读性好**。
4. C++生成的代码**质量高**，运行**效率高**，仅比汇编语言慢10%～20%；
5. C++更加安全，增加了const常量、引用、四类cast转换（static_cast、dynamic_cast、const_cast、reinterpret_cast）、智能指针、try—catch等等；
6. C++**可复用性**高，C++引入了**模板**的概念，后面在此基础上，实现了方便开发的标准模板库STL（Standard Template Library）。
7. 同时，C++是**不断在发展**的语言。C++后续版本更是发展了不少新特性，如C++11中引入了nullptr、auto变量、Lambda匿名函数、右值引用、智能指针。

#### 19、C++和Python的区别

包括但不限于：

Python是一种脚本语言，是解释执行的，而C++是编译语言，是需要编译后在特定平台运行的。python可以很方便的跨平台，但是效率没有C++高。

Python使用缩进来区分不同的代码块，C++使用花括号来区分

C++中需要事先定义变量的类型，而Python不需要

Python的库函数比C++的多，调用起来很方便

Python拥有很多的一些跑深度学习的框架，是深度学习研究领域不可或缺的语言。

#### 20、C语言和C++的区别

1. C语言是C++的子集，C++可以很好兼容C语言。但是C++又有很多**新特性**，如引用、智能指针、auto变量等。
2. C++是**面对对象**的编程语言；C语言是**面对过程**的编程语言。
3. C语言有一些不安全的语言特性，如指针使用的潜在危险、强制转换的不确定性、内存泄露等。而C++对此增加了不少新特性来**改善安全性**，如const常量、引用、cast转换、智能指针、try—catch等等；
4. C++**可复用性**高，C++引入了**模板**的概念，后面在此基础上，实现了方便开发的标准模板库STL。C++的STL库相对于C语言的函数库**更灵活、更通用**。

#### 21、C++与Java的区别

语言特性

Java语言给开发人员提供了更为简洁的语法；完全面向对象，由于JVM可以安装到任何的操作系统上，所以说它的可移植性强

Java语言中没有指针的概念，引入了真正的数组。

C++也可以在其他系统运行，但是需要不同的编码，Java程序一般都是生成字节码，在JVM里面运行得到结果

Java用接口(Interface)技术取代C++程序中的抽象类。接口与抽象类有同样的功能，但是省却了在实现和维护上的复杂性

**垃圾回收**

C++用析构函数回收垃圾

Java语言不使用指针，内存的分配和回收都是自动进行的

**应用场景**

Java在桌面程序上不如C++实用，C++可以直接编译成exe文件

Java在Web 应用上具有C++ 无可比拟的优势，具有丰富多样的框架

对于底层程序的编程以及控制方面的编程，C++很灵活，因为有句柄的存在。

#### 22、说说 C++中 struct 和 class 的区别

1. struct 一般用于描述一个数据结构集合，而 class 是对一个对象数据的封装；

2. struct 中默认的访问控制权限是 public 的，而 class 中默认的访问控制权限是 private 的，例如：

   ```C++
   struct A {  
   	int iNum; // 默认访问控制权限是 public 
   } 
   class B {  
       int iNum; // 默认访问控制权限是 private 
   }
   ```

3. 在继承关系中，struct 默认是公有继承，而 class 是私有继承；

4. class 关键字可以用于定义模板参数，就像 typename，而 struct 不能用于定义模板参数，例如：

   ```C++
   template<typename T, typename Y> // 可以把typename 换成class  
   int Func(const T& t, const Y& y) {      
       //TODO  
   }
   ```

#### 23、说说C++结构体和C结构体的区别

区别：

（1）C的结构体内不允许有函数存在，C++允许有内部成员函数，且允许该函数是虚函数。

（2）C的结构体对内部成员变量的访问权限只能是public，而C++允许public,protected,private三种。

（3）C语言的结构体是不可以继承的，C++的结构体是可以从其他的结构体或者类继承过来的。

（4）C 中使用结构体需要加上 struct 关键字，或者对结构体使用 typedef 取别名，而 C++ 中可以省略 struct 关键字直接使用。

- C++ 中的 struct 是对 C 中的 struct 进行了扩充，它们在声明时的区别如下：

|          |           C            |           C++            |
| :------- | :--------------------: | :----------------------: |
| 成员函数 |         不能有         |           可以           |
| 静态成员 |         不能有         |           可以           |
| 访问控制 |  默认public，不能修改  | public/private/protected |
| 继承关系 |       不可以继承       | 可从类或者其他结构体继承 |
| 初始化   | 不能直接初始化数据成员 |           可以           |

1. 使用时的区别：C 中使用结构体需要加上 struct 关键字，或者对结构体使用 typedef 取别名，而 C++ 中可以省略 struct 关键字直接使用，例如：

   ```C++
   struct Student {  
       int  iAgeNum;  
       string strName; 
   } 
   typedef struct Student Student2; //C中取别名  
   struct Student stu1; // C 中正常使用 
   Student2 stu2;   // C 中通过取别名的使用 
   Student stu3;   // C++ 中使用
   ```

#### 24、导入C函数的关键字是什么，C++编译时和C有什么不同？

1. **关键字：**在C++中，导入C函数的关键字是**extern**，表达形式为**extern “C”**， extern "C"的主要作用就是为了能够正确实现C++代码调用其他C语言代码。加上extern "C"后，会指示编译器这部分代码按**C语言**的进行编译，而不是C++的。

2. **编译区别：**由于C++支持函数重载，因此编译器编译函数的过程中会将函数的**参数类型**也加到编译后的代码中，而不仅仅是**函数名**；而C语言并不支持函数重载，因此编译C语言代码的函数时不会带上函数的参数类型，一般只包括**函数名**。

3. **哪些情况下使用extern "C"：**

   （1）C++代码中调用C语言代码；

   （2）在C++中的头文件中使用；

   （3）在多个人协同开发时，可能有人擅长C语言，而有人擅长C++；

#### 25、C和C++的类型安全

**什么是类型安全？**

类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域。“类型安全”常被用来形容编程语言，其根据在于该门编程语言是否提供保障类型安全的机制；有的时候也用“类型安全”形容某个程序，判别的标准在于该程序是否隐含类型错误。

**类型安全的编程语言与类型安全的程序之间，没有必然联系。**

（1）C的类型安全

C只在局部上下文中表现出类型安全，比如试图从一种结构体的指针转换成另一种结构体的指针时，编译器将会报告错误，除非使用显式类型转换。

（2）C++的类型安全

如果C++使用得当，它将远比C更有类型安全性。相比于C语言，C++提供了一些新的机制保障类型安全：

① 操作符new返回的指针类型严格与对象匹配，而不是void*

② C中很多以void*为参数的函数可以改写为C++模板函数，而模板是支持类型检查的；

③ 引入const关键字代替#define constants，它是有类型、有作用域的，而#define constants只是简单的文本替换

④ 一些#define宏可被改写为inline函数，会泽县类型检测

⑤ C++提供了dynamic_cast关键字，使得转换过程更加安全，因为dynamic_cast比static_cast涉及更多具体的类型检查。

#### 26、为什么C++没有垃圾回收机制？这点跟Java不太一样。

首先，实现一个垃圾回收器会带来额外的空间和时间开销。你需要开辟一定的空间保存指针的引用计数和对他们进行标记mark。然后需要单独开辟一个线程在空闲的时候进行free操作。

垃圾回收会使得C++不适合进行很多底层的操作。

#### 27、C++中新增了string，它与C语言中的 char \*有什么区别吗？它是如何实现的？

string继承自basic_string,其实是对char*进行了封装，封装的string包含了char*数组，容量，长度等等属性。

string可以进行动态扩展，在每次扩展的时候另外申请一块原空间大小两倍的空间（2^n），然后将原字符串拷贝过去，并加上新增的内容。

#### 28、cout和printf有什么区别？

cout<<是一个函数，cout<<后可以跟不同的类型是因为cout<<已存在针对各种类型数据的重载，所以会自动识别数据的类型。

输出过程会首先将输出字符放入缓冲区，然后输出到屏幕。

flush立即强迫缓冲输出。

printf是行缓冲输出，不是无缓冲输出。

#### 29、C++的异常处理的方法

常见的异常有：数组下标越界、除法计算时除数为0、动态分配空间时空间不足...

（1）try、throw和catch关键字

C++中的异常处理机制主要使用try、throw和catch三个关键字，其流程是：

先执行try包裹的语句块，如果执行过程中没有异常发生，则不会进入任何catch包裹的语句块，如果发生异常，则使用throw进行异常抛出，再由catch进行捕获

（2）函数的异常声明列表

程序员在定义函数的时候知道函数可能发生的异常，可以在函数声明和定义时，指出所能抛出异常的列表，如：int fun() throw(int,double,A,B,C){...};

（3）C++标准异常类 exception

C++ 标准库中有一些类代表异常，这些类都是从 exception 类派生而来的：

① bad_typeid：使用typeid运算符，如果其操作数是一个多态类的指针，而该指针的值为 NULL，则会拋出此异常

② bad_cast：在用 dynamic_cast 进行从多态基类对象（或引用）到派生类的引用的强制类型转换时，如果转换是不安全的，则会拋出此异常

③ bad_alloc：在用 new 运算符进行动态内存分配时，如果没有足够的内存，则会引发此异常

④ out_of_range：如果下标越界，则会拋出此异常

#### 30、为什么C++没有垃圾回收机制？这点跟Java不太一样。

首先，实现一个垃圾回收器会带来额外的空间和时间开销。你需要开辟一定的空间保存指针的引用计数和对他们进行标记mark。然后需要单独开辟一个线程在空闲的时候进行free操作。

垃圾回收会使得C++不适合进行很多底层的操作。

### 三、程序编译、执行

#### 30、**在main执行之前和之后执行的代码可能是什么？**

main函数执行之前：

① 设置栈指针

② 初始化全局变量和静态变量即.data段内容

③ 将未初始化的全局变量赋初值，如short、int、long为0，bool为false，指针为NULL等，即.bss段内容

④ 全局对象初始化，在main函数之前调用构造函数，这是可能会执行前的一些代码。

⑤ 将mian函数的参数argc、argv传递给main，然后才真正运行main函数

⑥ _attribute_((constructor))

main函数执行之后：

① 全局对象的析构函数会在main函数之后执行。

② 可以用atexit注册一个函数，它会在main函数之后执行。

③ _attribute_((destructor))

#### 31、简述C++从代码到可执行二进制文件的过程

​    C++和C语言类似，一个C++程序从源码到执行文件，有四个过程，**预编译、编译、汇编、链接**。

1. 预编译：这个过程主要的处理操作如下：

   （1） 将所有的#define删除，并且展开所有的宏定义

   （2） 处理所有的条件预编译指令，如#if、#ifdef

   （3） 处理#include预编译指令，将被包含的文件插入到该预编译指令的位置。

   （4） 过滤所有的注释

   （5） 添加行号和文件名标识。

2. 编译：这个过程主要的处理操作如下：

   （1） 词法分析：将源代码的字符序列分割成一系列的记号。

   （2） 语法分析：对记号进行语法分析，产生语法树。

   （3） 语义分析：判断表达式是否有意义。

   （4） 代码优化：

   （5） 目标代码生成：生成汇编代码。

   （6） 目标代码优化：

3. 汇编：这个过程主要是将汇编代码转变成机器可以执行的指令。

4. 链接：将不同的源文件产生的目标文件进行链接，从而形成一个可以执行的程序。

   链接分为<u>静态链接和动态链接</u>。

   静态链接，是在链接的时候就已经把要调用的函数或者过程链接到了生成的可执行文件中，就算你在去把静态库删除也不会影响可执行程序的执行；生成的静态链接库，Windows下以.lib为后缀，Linux下以.a为后缀。

   - **更新困难**：每当库函数的代码修改了，这个时候就需要重新进行编译链接形成可执行程序
   - **运行速度快**：但是静态链接的优点就是，在可执行程序中已经具备了所有执行程序所需要的任何东西，在执行的时候运行速度快。

   动态链接，是在链接的时候没有把调用的函数代码链接进去，而是在执行的过程中，再去找要链接的函数，生成的可执行文件中没有函数代码，只包含函数的重定位信息，所以当你删除动态库时，可执行程序就不能运行。生成的动态链接库，Windows下以.dll为后缀，Linux下以.so为后缀。

   - **共享库**：就是即使需要每个程序都依赖同一个库，但是该库不会像静态链接那样在内存中存在多分，副本，而是这多个程序在执行时共享同一份副本；
   - **更新方便**：更新时只需要替换原来的目标文件，而无需将所有的程序再重新链接一遍。当程序下一次运 行时，新版本的目标文件会被自动加载到内存并且链接起来，程序就完成了升级的目标。
   - **性能损耗**：因为把链接推迟到了程序运行时，所以每次执行程序都需要进行链接，所以性能会有一定损失。

#### 32、**动态编译与静态编译**

**静态编译**，编译器在编译可执行文件时，把需要用到的对应动态链接库中的部分提取出来，连接到可执行文件中去，使**可执行文件在运行时不需要依赖于动态链接库**；

**动态编译的**可执行文件需要附带一个动态链接库，在执行时，需要调用其对应动态链接库的命令。所以其优点一方面是缩小了执行文件本身的体积，另一方面是加快了编译速度，节省了系统资源。缺点是哪怕是很简单的程序，只用到了链接库的一两条命令，也需要附带一个相对庞大的链接库；二是如果其他计算机上没有安装对应的运行库，则用动态编译的可执行文件就不能运行。



####  

