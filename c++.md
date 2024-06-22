## 一章：c++基础

### 面向过程和面向对象

面向过程编程的主体是函数，他以函数为中心，自上而下，顺序执行，逐步细化

面向过程的优点：性能比面向对象高

​					缺点：没有面向对象易维护，易复用，易扩展

面向对象编程是以对象为中心，他要解决的问题分解成各个对象（变量，数据）而不是各个流程，步骤，更注重对象与对象的交互（而不是方法与方法，数据与方法），步骤是分析参与问题的有哪些实体，这些实体应该有什么属性和方法，如何通过调用这些实体的属性和方法解决问题

面向对象的优点：易维护，易复用，易扩展，由于面向对象有封装，继承，多态的特征，可以设计出低耦合的系统，使系统更加灵活，更加易于维护

​					缺点：性能比面向过程低，因为类调用时需要实例化，开销比较大，消耗资源

### new delete 与 malloc() free() 区别

1.new delete是关键字，malloc() free()是库函数，需要头文件支持

2.new申请的空间大小无需指定，系统根据类型自行计算，new返回的是申请类型的地址，不需要强转。malloc()需要显示指定所申请空间的大小，且返回void*，因此需要强转为所需类型

3.new申请空间的同时可以进行初始化

```c++
//初始化
char *p1=new char('a');
int *p2=new int[3]{1,2,3};
//new 整型地址
int** p=new int *;
delete p;
p=nullptr;
//new指针数组
int** p=new int* [3];
delete[] p;
p=nullptr;
//new数组指针
int(**p)[5]=new(int(*)[5]);
delete p;
p=nullptr;
//new二维数组
int(*p)[2]=new int{{1,2},{3,4},{5,6}};
delete[]p;
p=nullptr;
//new函数指针
void (**p)(int) = new (void (*)(int));
```

4.new申请类，结构体对象内存空间会自动调用构造函数，delete会自动调用类，结构体的析构函数，而malloc(),free()不会



### 字符串

#### char*p与char ch[]的区别：

```c++
//不可用指针改变字符串中的成员，因为字符串是常量
char*p="abcd";
p=(char*)"qwer";
//p[1]='t';//不可以改变

//可以改变字符串中的成员，因为此时的字符串是从常数区复制到栈区，但是不可以用数组名进行赋值，因为数组名是地址常量
char ch[5]={"abcd"};
ch[0]='h';
//ch="grgr";//不可以修改
```

#### 字符串一些库函数的功能：

```c++
char *strcpy( char *to, const char *from );
//功能：复制字符串from 中的字符到字符串to，包括空值结束符。返回值为指针to。

char *strcat( char *str1, const char *str2 );
//功能：函数将字符串str2 连接到str1的末端，并返回指针str1.

int strcmp( const char *str1, const char *str2 );
//小于0：前小于后
//大于0：前大于后
//等于0，前等于后

char *strstr( const char *str1, const char *str2 );
//功能：函数返回一个指针，它指向字符串str2 首次出现于字符串str1中的位置，如果没有找到，返回NULL。
```

#### string类型的字符串

```c++
string s="afsdgfdgdfthgy"
//字符串截取
cout<<s.substr(2,4)<<endl;//(初始位置，长度) 
//将string转换成char*
cout<<s.c_str()<<endl;
printf("%s",s.c_str());
//删除字符串
s1.erase(0,2);//从pos开始的len字符
```

### 各种类型的增强的范围for

```c++
int arr[5]={1,2,3,4,5};
for(int v:arr)
{
    cout<<v<<endl;
}
```

### 函数

#### 函数默认参数值

```c++
//如果函数声明和定义分开写，一般在函数声明处指定默认值
//在代码执行的时候，参数数量取决于函数声明处
void fun(int a=1);
void fun(int a)
{
    cout<<__FUNCTION__<<" "<<a<<endl;
}

//多次声明，可指定一片区域，在此区域内声明并指定默认值
{
    void fun(int a=5);
    fun();
}
```

#### 函数重载



函数重载：在同一个作用域，函数名相同，参数列表（参数类型，数量，顺序）不同，与返回类型无关（可同可不同），这样的函数即为函数重载

注意：

1.若函数参数是char*p 与char p[]，不是函数重载，是函数重定义

2.函数参数为const int 与int时不构成重载，构成重定义

​	而int* 与const int*会构成函数重载

3.值传递，地址传递，引用传递的函数传参方式构成重载关系

4.判断题：一个类中定义了两个同名且参数列表也相同的函数，但其中一个是静态成员函数。

5。在类中一般函数和静态成员函数不构成重载，为重定义

​	**错误，同时存在即是错误**

5.例题：区分int add(int a, int b); 与 int add(int &a, int &b );

​	**方法一：作用域局部声明**

```c++
int a=10,b=20;
{
    int add(int a,int b);
    cout<<add(a,b)<<endl;
}
```

​	**方法二：函数指针**

```c++
int a=10,b=20;
int(*p_fun1)(int,int)=&fun;
int(*p_fun2)(int&,int&)=&fun;
p_fun1(a,b);
p_fun2(a,b);
```

6.

```c++
void fun(int a){
	cout << __FUNCSIG__ << endl;
}
void fun(int a, char b = 'b'){
	cout << __FUNCSIG__ << endl;
}

int main(){
    //fun(10) // 会有歧义
    void fun(int a);// 函数局部声明
    fun(10);
    
    {
        void fun(int a, char b = 'c');//默认值可以修改
        fun(10);
    }
    return 0;
}
```



### NULL和nullptr的区别

NULL：宏替换了数字0

nullptr:关键字，一个明确的含义，空指针

​			 解决了在函数重载的情况下，数字0含义不明确（整型0和空指针混淆）的问题

### 引用和指针的区别

##### 引用介绍

```c++
/*
引用：对一块已存在的空间（变量）起了一个别名
如果是查看，都可以
如果要修改实参，不能采用值传递，可以选择引用、地址传递
推荐使用引用传递，不额外申请空间
指针额外申请指针大小的空间（可控）
值传递额外申请空间大小不确定，要根据类型才能确定其大小
*/
void fun(int &c){
    cout << 1 << endl;
    c = 30;
}
void fun(int c){
	cout << 2 << endl;
}
void (*p_fun1)(int) = &fun;
void (*p_fun2)(int &) = &fun;
int main(){
    int a = 10;
    int& b = a;
    void fun(int& a);
    fun(a);
    cout << a << endl;// 30
    (*p_fun1)(a);
    (*p_fun2)(b);
    return 0;
}
// 1 30 2 1
```







##### 指针和引用的区别

1.引用定义了就要初始化，指针可不用初始化（但不推荐）

2.引用初始化后就不能再重新引用其他的变量、空间，指针可以随意指向。

3.有NULL指针，没有NULL的引用。

4.指针变量额外占用空间，引用不额外申请空间。

5.指针可以有多级，但引用只能有一级。



6.指针在使用时需要解引用

7.指针自增指向下一个地址，引用自增是引用变量值+1

```c++
int a=10;
int& b=a;//初始化引用

b=20;
cout<<a<<endl;//20;

int aa=1;
b=aa;//赋值操作

```



## 第二章：类基础

### 类和结构体区别：

1.类成员属性、方法访问修饰符默认是私有的(private)，而结构体默认是公有的(public)。

2.当从基类、结构体中继承时，类的默认继承方式是私有的(私有)，而结构体是公有的(公共)。此外在使用习惯上看也略有差别:一般情况，使用类来描述功能实现，而结构体通常只是纯粹的表示数据.比如链表有复杂的数据、很多的方法一般定义成类，而节点只是描述了持有数据的类型和指向下一个节点的指针，一般被定义为结构体.

### 类和对象：

#### 类和对象的定义：

类（class）：完成某一功能的数据和算法的集合，是一个抽象的概念。

对象：类的一个实例，具体的概念，是真正存在于内存中的。

#### 构造注意：

1. 类名(参数)，无返回类型如果是编译器默认提供的则为无参，如果手动重构，参数为人为指定
2. 什么时候调用：定义对象时，由编译器自动调用匹配的构造函数
3. 用处：用来初始化类中成员属性
4. 多个构造函数之间的关系：函数重载
5. 一旦我们手动重构了任何构造函数，编译器就不会提供默认的构造函数了

#### 析构注意：

当对象的生命周期结束时（即将被销毁），在回收对象空间**之前**，先调用析构函数用来回收类中成员额外申请的空间，编译器再去回收对象本身

1. ~类名()，参数都为空，无参，函数体代码：编译器默认提供的为空
2. 什么时候调用：当对象生命周期结束时（即将被销毁），由编译器自动调用
3. 用处：回收类中成员额外申请的空间
4. 一个类中析构函数，只能有一个
5. 一旦我们手动重构了析构函数，编译器就不会提供默认的无参析构函数了

## 第三章：类成员进阶

### 类成员：

#### 空类大小：

一个空类即使没有任何数据存储其大小也不能为0，空类实例占用的大小为1，用来在内存中占位

#### 成员：

- 成员属性：属于对象的，当定义对象时，属性才会存在，才会开辟对应的空间，多个对象会存在多份的成员属性，彼此独立互不干扰
- 成员函数：属于类的，编译期存在，一个类只会存在一份，它的存在与否和是否定义对象无关

创建多个对象，对象成员属性的地址不同

属于对象的成员属性，在定义对象时才会存在

### this指针：

由编译器默认添加到类中的非静态成员函数

作用：指向了调用该函数的对象，在函数中使用其他成员都是通过this指针间接使用的

类型是CTest * const this

this指针既是指针变量，也是关键字

```c++
//类中一般成员函数，默认会存在一个隐藏的参数，this指针，类型，类* const，由编译器默认添加的第一个参数
//作用：指针指向了调用该函数的对象，函数中使用类成员都是通过this指针去调用的，连接对象与成员的桥梁
class CTest{
public:
    int m_a;
    void fun(CTest* const this){
        cout << __FUNCTION__ << m_a << endl;
        cout << this->m_a << endl;
        show();
        this->show();//与上句等效
    }
    void show(){
        cout << __FUNCTION__ << endl;
    }
};
```



### 静态成员：

#### 静态成员属性:

特点：

静态成员属性属于类，不参与对象的空间占用，在编译期就存在

需要在类外进行初始化，省略static关键字

可直接类名作用域去调用，也可以调用对象去调用

```c++
class CTest {
public:
	int m_a;
	static int m_b;
};
int CTest::m_b=10;

```

#### 静态成员函数：

属于类的，编译期存在，一个类中存在一份，被多个对象所共享，存在与否与是否定义对象无关

在没有对象的情况下，仍然可以使用静态成员函数

如果存在对象，也可以通过对象去使用

注意：静态成员函数不能使用一般的成员函数



一般的成员函数和静态成员函数的区别：

1.静态成员函数 没有隐藏的this指针参数，一般的成员函数有。

2.静态成员函数 只能使用静态的成员，而一般的成员函数都可以使用。

3.静态成员函数 是否通过对象调用都可以，一般的成员函数必须通过对象调用。

### const常量，常函数和常量对象

#### 常量：

带有const的变量在定义的时候必须要初始化，且初始化是在**初始化参数列表**中完成的，不能在函数体内部进行赋值操作

```c++
class CTest{
public:
	int m_a;
	const int m_b;
	CTest():m_a(2), m_b(3){}
    //初始化参数列表初始化成员的顺序取决于：成员在类中声明的顺序
    CTest(int a):m_a(a), m_b(m_a){}//m_a与m_b初始化顺序反过来则不行
};
```



#### 常函数

##### 特性

```c++
class CTest{
public:
	int m_a;
	const int m_b;
    mutable int m_c;// 修饰的当前变量在常函数中可直接修改
	static int m_d;
    //常函数特性：不能修改类中的非静态成员
	void fun(/*const CTest* const this*/) const{
        //m_a = 10;//变量不可修改
        //m_b = 20;//常量不可修改
        m_c = 30;
        m_d = 40;//在长函数中可以修改静态属性
    }
};
int CTest::m_d = 1;
```

常量指针升级降级

```c++
int* p = nullptr;
const int* p2 = p;// int*->const int* ,指针升级合法
//----------------------------------------
const int a = 0;
p = &a; // 非法
//----------------------------------------
//常函数不能调用普通的类成员函数（指针降级）

```

- 常函数不能调用普通的类成员函数（指针降级）
- 普通的类成员函数可以调用常函数
- 常函数能调用静态成员函数，但是静态成员函数不能调用普通成员函数以及常函数

##### 常函数与普通函数的区别

1. 本质上的区别：
   常函数 this指针的类型：const 类 * const this
   普通函数 this指针的类型：类 * const this
2. 常函数不能修改类中成员属性（除mutable），也不能调用一般的函数
   一般的函数可以修改类成员变量，也可以调用常函数

#### 常量对象

常量对象只可以调用常函数

## 第四章：类成员关系

### 继承：

可以将类中的一些功能相近相似的共同的方法，抽离出来放到单独的一个类中，提高了代码的复用性，扩展性

#### 继承的优点

- 将一些功能比较相似的类中公共的成员，抽离出来封装成一个类，为父类。减少代码的冗余，提高代码的复用性、扩展性

#### 内存空间：

子类继承父类后，成员在内存空间分布为：先父类成员后子类成员

#### 构造析构顺序：

构造：先父后子

析构：先子后父

```
构造执行的顺序:父类构造-子类构造
定义子类对象，优先调用的是子类的构造，在子类的初始化参数列表初始化父类和子类的成员，先调用父类的构造函数初始化父类成员，再在初始化子类自己的成员(同内存排布顾序一致)

析构顺序:子类析构->父类析构:定义子类对象，当子类对象生命周期结束时，优先匹配的子类的析构函数，回收完子类申请的额外空间后，在回收对象本身的空间。
子类对象中包含了父类匿名对象，在回收这个匿名对象前，先调用父类的析构，在回收父类n对象
```

- 子类构造初始化参数列表，由编译器默认调用父类无参构造
- 如果想调用父类带参数的构造，或者父类没有无参数构造，则必须显式指定父类构造

#### 继承方式：

public

|   父类    |   子类    |
| :-------: | :-------: |
|  public   |  public   |
| protected | protected |
|  private  | 不可访问  |

protected

|   父类    |   子类    |
| :-------: | :-------: |
|  public   | protected |
| protected | protected |
|  private  | 不可访问  |

private

|   父类    |   子类   |
| :-------: | :------: |
|  public   | private  |
| protected | private  |
|  private  | 不可访问 |



#### 隐藏：

父类和子类中中同名的函数存在互为隐藏的关系，要想使用父类需要显示指定父类类名作用域。

若子类无对应的函数参数列表，则父类被隐藏



```c++
class CFather {
public:
	void fun(/* CFather* const this  */int a, int b) {
		cout << __FUNCTION__ << "  " << a << "  " << b << endl;
		cout << this << endl;
	}
 
};
 
class CSon : public CFather {
public:
	//void fun() {
	//	cout << __FUNCTION__ << endl;
	//}
 
	//void fun(int a) {
	//	cout << __FUNCTION__ << "  " << a << endl;
	//}
};
	CSon son;
	son.fun(10, 20); 
	//在继承的条件下，父类指针可以直接指向子类对象（不通过强转），保证了子类对象可以调用父类函数
	CFather* const pthis = &son; 
	cout << pthis << endl;

	//CFather fa;
	//CSon* cs = &fa;// error
	//子类指针不能指向父类对象
//测试：父类指针指向子类的地址和父类中函数成员自身的this指针指向的地址相同。
```



当子类函数名与父类函数名相同时，子类的函数将父类函数隐藏，不能使用父类的函数



::*是c++的整体操作符，作用是定义类成员函数指针

.* 通过对象调用类成员函数指针指向的函数

### 类成员函数指针：

#### 普通的函数指针：

```c++
void fun()
{
    cout<<__FUNCTION__<<endl;
}
void (*p_fun)()=&fun();
(*p_fun)();

typedef void(*P_FUN)();
P_FUN p_fun1=&fun;
(*p_fun1)();
```

#### 类成员函数指针：

```c++
class CTest {
public:
	void fun(/* CTest * const this */) {
		cout << __FUNCTION__ << endl;
	}
};
//全局函数与类成员函数的区别
//1、作用域不同
//2、类成员函数有隐藏的this指针作为第一个参数，而全局函数没有
void fun(){
    cout << __FUNCTION__ << endl;
}
//----------------------------------------------
// 确保类型相同
// 类成员函数指针： ::*是c++的整体操作符，作用是定义类成员函数指针
void (CTest::*p_fun3) () = &CTest::fun;
(tst .* p_fun3)();  

CTest* pTst1 = new CTest;
CTest pTst2;
// 通过对象调用类成员函数指针指向的函数
(pTst->*p_fun3)();
(pTst2.*p_fun3)();
// 普通函数指针
void (*p_fun)() = &fun;
(*p_fun)();
```

#### 父类指针调用子类对象：

```c++
class CFather {
public:
 
};
class CSon : public CFather {
public:
	void fun() {
		cout << __FUNCTION__ << endl;
	}
};
	//在继承关系下，允许父类的指针指向子类的对象
	//这样做的好处是，父类指针可以统一多个类的类型，提高了代码的复用性，扩展性
	CFather* pFa = new CSon;
	//pFa->fun();
 
	//-------------------------------
	void(CFather :: * p_fun)() = (void(CFather :: * )())&CSon::fun;
	(pFa->*p_fun)();
```

可以进行隐式转换

::*是c++的整体操作符，作用是定义类成员函数指针

.* 通过对象调用类成员函数指针指向的函数

## 第五章：多态

### 多态定义：

#### 多态：

- 相同的行为方式导致了不同的行为结果，同一行语句展现出多种不同的表现状态，即多态性
- 在继承的条件下，父类指针可以指向任何继承于该类的子类对象，多种子类具有多种形态由父类的指针统一管理，那么父类的指针也会有多种形态，即c++多态

#### c++支持多态条件：

1. 在继承状态下，父类指针指向子类对象，通过该指针调用虚函数
2. 父类中存在虚函数，且子类中重写了父类的虚函数

重写：在继承条件下，在父类虚函数存在的前提下，子类定义了一个与父类虚函数一模一样的函数

子类的virtual可以省略

#### 多态优点：

解决了继承中的问腿，可以通过父类指针准确调用子类中不统一的函数，且无需使用函数指针

### 虚函数-虚函数指针-虚函数列表

#### 虚函数：

普通的函数不会占用类的空间，若在空类中创建一个或多个虚函数，类所占的空间则是**4个字节**

#### 虚函数指针：

- __vfptr （虚函数指针）：在一个类中，当存在虚函数时，在定义对象的内存空间的首地址会多分配出一块内存，在这块内存中增加一个指针变量（二级指针 void**），也就是虚函数指针。

- 它指向的是一个void*的数组，多个对象中多份虚函数指针指向的是同一个地址（同一个数组）
- 当类中存在虚函数时，编译器就会默认添加虚函数指针

注意：

1.属于类的，在编译期存在，与所有成员共享

2.必须通过真实存在的对象调用，无对象或者空指针对象无法调用虚函数

#### 虚函数调用流程：

1.定义对象获取对象内存首地址中的__vfptr。

2.通过指针间接引用，找到虚函数指针指向的虚函数列表vftable。

3.通过下标定位到要调用的虚函数元素（虚函数地址）。

4.通过这个地址（函数入口地址）调用到了虚函数。

#### 虚函数列表：

本质上是虚函数的函数指针数组，每一个元素存储的是虚函数的地址，顺序为虚函数在类中定义顺序，在编译器就存在，是属于类的

#### 虚函数和普通成员函数的区别：

1.调用流程不同：虚函数的调用流程相比普通成员函数而言复杂得多，这是他们的本质区别。

2.调用效率不同：普通的成员函数通过函数名（即函数入口地址）直接调用执行函数，效率高速度快，虚函数的调用需要虚函数指针-虚函数列表的参与，效率低，速度慢。

3.使用场景不同：虚函数主要用于实现多态，这一点是普通函数无法做到的。

### 多态实现的原理：

**前提：**

虚函数列表是属于类的，父类和子类都会有各自的虚函数列表，vfptr属于对象，每个对象都有各自的_vfptr

**原理：**

1.由于子类继承父类，不但继承了父类的成员，还会**继承父类的虚函数列表**

2.编译器会检查子类是否重写父类的虚函数，如果重写，子类的虚函数会覆盖掉父类的虚函（原位置替换），覆盖后便指向了子类的虚函数

3.如果子类没有重写父类的虚函数，父类虚函数会保留在子类的虚函数列表

4.如果子类定义了独有的虚函数，将其添加到子类的虚函数列表末尾

**流程：**

```c++
class CFather
{
public:
	virtual void fun1() { cout << "CFather::fun1" << endl; }
	virtual void fun2() { cout << "CFather::fun2" << endl; }
};
class CSon :public CFather {
public:
	virtual void fun2() { cout << "CSon::fun2" << endl; }
	virtual void fun3() { cout << "CSon::fun3" << endl; }
};

int main()
{
    //定义（new）哪个子类对象，虚函数指针就会指向哪个类的虚函数列表，与哪个指针无关
	CSon son;
	CFather fa;
    //虚函数指针->子类的虚函数列表（而不是父类）
	CFather* pfa = new CSon;
	int* p = (int*)pfa;

	pfa->fun1();//CFather::fun1
	pfa->fun2();//CSon::fun2
	typedef void(* P_FUN)();
	P_FUN p_fun1 = (P_FUN)((int*)*(int*)p)[0];//CFather::fun1
	P_FUN p_fun2 = (P_FUN)((int*)*(int*)p)[1];//CSon::fun2
	P_FUN p_fun3 = (P_FUN)((int*)*(int*)p)[2];//CSon::fun3
	(*p_fun1)();
	(*p_fun2)();
	(*p_fun3)();
	return 0;
}
```

### 虚析构：

#### 问题：

在多态下，父类的指针指向子类的对象，最后在回收空间的时候，是按照父类的类型来回收的，只调用了父类的析构，子类的析构并没有执行，可能会导致内存泄漏

delete调用哪个类析构，取决于后面放的指针类型，与指针指向的对象类型无关

```c++
CFather* pfa = new CFather;
delete pfa;//若为虚析构，先执行子类析构，后执行父类析构
```



#### 虚析构解决：

在父类析构函数之前加~使其变为虚析构时，调用pFa析构会发生多态行为，真正调用的是子类析构，最后回收对象内存空间时，再调用父类的析构

### 纯虚函数：

在多态下，有时抽象出来的虚函数作为接口函数，并不知道如何实现或者不需要实现，就是为了多态而生的，只有继承的子类才知道如何实现，可以把父类的虚函数变为纯虚函数

**写法：**在一般虚函数后=0

**特点：**当前类不必实现，而子类**必须要重写**实现父类的纯虚函数，如果父类的纯虚函数没有被重写，**不允许**定义子类对象

抽象类：包含纯虚函数的类

接口类：所有的虚函数都是纯虚函数

### 虚函数实现多态的缺点：

1.效率问题：调用虚函数效率低，速度慢

2.空间问题：定义每一个对象都会额外开辟指针大小的空间，虚函数列表占用程序的内存空间，并且会随着继承的层级递增，占用的空间越来越多

3.安全问题：通过其他方法可以模拟虚函数的调用，跨过访问修饰符的限制，**私有的函数最好不要变为虚函数，否则会有安全隐患**

## 第六章：探索程序

### 头文件-源文件

#### 区别：

1.默认情况下，头文件不参与编译，而每个源文件自上而下独立编译

2.头文件存放：变量，类型，函数的声明；宏，结构体和类的定义

​	源文件存放：变量的定义初始化，函数的定义实现放于源文件中

```c++
//.h
extern int a;
//.cpp
int a=1;
```

### 各类型变量的声明定义：

```c++
//常函数
void fun() const;
void CTest::fun() const{}
//静态成员函数
static void fun();
void CTest::fun(){}
//虚函数
virtual void fun();
void CTest::fun(){}
//纯虚函数（）不需要实现
virtual void fun()=0;
```

#### 总结：

类中的成员函数在对应的源文件中定义时，一定要加上类名作用域

类中普通成员属性和const成员属性在构造函数中的初始化参数列表进行初始化定义，而静态成员属性要在源文件中单独定义

静态成员函数，虚函数定义去掉关键字

### 头文件重复包含：

#### 问题:

如果在一个头文件中创建一个类，然后另外两个头文件又都用到了这个头文件，在源文件中使用了另外两个头文件，那么第一个头文件中的类就会被创建两次，会出现重定义的错误。

#### 解决方案：

1.#pragma once:告诉编译器，当前的头文件在其他的源文件中只包含一次

2.宏逻辑判断

#### 对比：

1.#pragma once相对来说效率高

2.基于宏逻辑判断，在大量头文件时，编译速度降低，耗时增加，而且需要考虑宏重名的问题。

```c++
#ifndef __AA_H__
#define __AA_H__
class AA{
public:
	int m_a;
    AA():m_a(0){}
};
#endif
```



### 程序生成过程：

#### 程序生成过程：

**预处理：**

将原文件（.cpp）初步处理，生成预处理文件（.i）：

1.解析#include头文件展开替换。
2.宏定义指令：#define宏的替换，#undef等。
3.预处理指令：解析#if、#ifndef、#ifdef、#else、#elif、#endif等。
4.删除所有注释。 
**编译期**
将预处理后的文件（.i）进行一系列词法分析、语法分析、语义分析及优化，产生相应的汇编代码文件（.asm）。

**汇编期**
将编译后的汇编代码文件（.asm）汇编指令逐条翻译成目标机器指令，并生成可重定位目标程序的.obj文件，该文件为二进制文件，字节编码是机器指令。

**链接期**
通过链接器将多个目标文件（.obj）和库文件链接在一起生成一个完整的可执行程序。

#### 编译期运行期：

**编译期**：是指把源程序交给编译器编译、生成的过程，最终得到可执行文件。
编译期限制访问修饰符，作用域

**运行期**：是指将可执行文件交给操作系统执行、直到程序退出，把在磁盘中的程序二进制代码放到内存中执行起来，执行的目的是为了实现程序的功能。

运行期限制指针，引用对象

**注意：**数组越界是运行期错误，用变量定义数组是编译期错误

```c++
	int len = 20;
	int arr[len];  //编译期错误
//-------------------------------------
    int len = 0;
	cin >> len;
	int* p = new int[len];  //运行期 
	//p[100] = 10; //数组越界:运行期错误
```

#### 类和对象：

类：编译期的概念，包括类成员函数，静态属性，作用域，访问修饰符

对象：运行期的概念实例，引用，指针

```c++
class CFather {
public:
	virtual void fun() {
		cout << "CFather::fun" << endl;
	}
};
 
class CSon :public CFather {
private: //编译期的限制
	virtual void fun() {
		cout << "CSon::fun" << endl;
	}
};
	CFather* pFa = new CSon;
 
	pFa->fun();   //Cson::fun
```

private是编译期的限制，在运行时无效，可以调用子类的fun函数

### 宏：

#### 宏替换优缺点：

**优点：**

1. 使用宏可以替换在程序中经常使用的常量或表达式，在后期程序维护时，不用对整个程序进行修改，只需要维护、修改一份宏定义的内容即可。
2. 宏在一定程度上可以代替简单的函数，这样就省去了调用函数的各种开销，提高程序的运行效率。

**缺点：**

1. 不方便调试。
2. 没有类型安全的检查
3. 对带参的宏而言，由于是直接替换，并不会检查参数是否合法，也并不会计算求解，存在一定的安全隐患。

#### 宏的用法：

**#undef：**取消宏定义

```c++
#define A 10
cout<<a<<endl;
#undef A//取消宏定义

	int A=20;
	cout<<A<<endl;
```



```c++
#define N  10
// \ 作用：连接本行与下一行，一般最后一行变成\
// \ 后面不能有任何的字符，包括空格，注释等
#define M \
	for(int i = 0;i < N;i ++){\
		cout << i << " ";\
	}\
	cout << endl;

#define K(NUM)\
	for(int i = 0;i < NUM;i ++){\
		cout << i << " ";\
	}\
	cout << endl;

//#将宏参数转为字符串，相当于加了双引号
#define D(PARAM) #PARAM
fun(D(123));

//#@将宏参数转为字符，相当于加了单引号
#define D(PARAM) #@PARAM
fun(D(1));

//##起拼接作用
#define D(PARAM) int a##PARAM =100;
	D(2);
	cout<<a2<<endl;

#define MUL(A,B) ((A)*(B))
// 宏无法调试，没有像函数一样，做安全检查类型的检查等
// 同等条件下，宏的效率比函数高
//------------------------------------------------
//当函数名与宏名相同时，默认情况下使用的是宏，若想使用函数，将函数名用括号括起来
//MUL(1, 2);
//(MUL)(1, 2);

```

### inline内联函数注意：

​		内联是以**代码膨胀（复制）为代价，空间换时间，仅仅省去了函数调用的开销，从而提高函数的执行效率**。 如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。

​		类、结构中在的类内部声明并定义的函数默认为内联函数，如果类中只给出声明，在类外定义的函数，那么默认不是内联函数，除非我们手动加上inline关键字。

以下情况不宜使用内联： 
(1) 如果函数体内的代码比较长，使用内联将导致内存消耗代价较高。 
(2) 如果函数体内出现循环，那么执行函数体内代码的时间要比函数调用的开销大。
(3) 类的构造函数和析构函数容易让人误解成使用内联更有效。

## 第七章：重载操作符

### 重载操作符要点：

```
本质上是一个函数，告诉编译器，当遇到这个操作符时，能够匹配重载的这个操作符函数，通过函数来实现了这个操作符的功能
注意：参数匹配，类型，顺序，数量都需匹配才能够调用
```

不能重载的运算符：长度运算符 sizeof、三目运算符 ? :、成员选择符 . 、作用域运算符 :: 等。
还有一些操作符只能在类内重载，**赋值=，下标[]，调用()，成员指向->**，操作符必须被定义为类成员操作符。
重载操作符不能改变操作符的用法，原来有几个操作数、操作数在左边还是右边，这些都不会改变。
运算符重载函数**不能有默认的参数**，否则就改变了运算符操作数的个数，这显然是错误的。
重载操作符不**能改变运算符的优先级和结合性**。
不能创建新的运算符。

### 举例：

```c++
class CTest{
public:
	int m_a;
    CTest():m_a(1){}
    // 函数名字 operator关键字后，加上重载的操作符号，作为一个函数名，参数按照操作符的使用规则来设定
    // 返回类型：一般是要有的，为了和后续的操作符继续操作
    int operator+(/*CTest* const this, */int a){
        return this->m_a + a;
    }
    int operator=(int a){
        this->m_a = a;
        return m_a;
    }
};
int operator+(int a, CTest& tst){
    return a + tst.m_a;
}
int main(){
    CTest tst;
    int ret = tst + 2;
    cout << ret << endl;
    tst.operator+(4);// 可以像普通的类成员函数一样，直接通过对象去调用，但是最好通过操作符自动匹配函数
 	tst = ret;
    return 0;
}
```

### 类内重载与类外重载

1. 同一个操作符在类外重载会比类内重载的函数多一个参数
2. 类外重载函数，参数顺序可自定义设定，类内参数顺序相对比较固定
3. 类内和类外重载操作符函数注意是否产生二义性

```c++
tst.operator+(4);// 通过对象调用
::operator(tst, 4);// 指定全局的函数调用

//类内重载
int operator*();// 间接引用
int operator*(int);// 乘法
```



### 左++右++

```c++
class CTest
{
public:
    int m_a;
  	int operator++()
    {
        return ++this->m_a;//左++
    }
    int operator++(int)//在参数上增加一个整型，起区分作用
    {
        return this->m_a++;//右++
    }
};
```

### 自定义重载输入输出操作符：

```c++
ostream& operator<<(ostream& os,CTest& tst)
{
    os<<tst.m_a;
    return os;
}
istream& operator>>(istream& is,CTest& tst)
{
    is>>tst.m_a;
    return is;
}
```

### 对象类型转换

```c++
class CTest
{
public:
	int m_a;
	CTest():m_a(1){}
	//对象类型转换，重载类型，将对象临时的当成在这个类型用一下
	operator int()//无参，无返回类型，像构造一样真的没有返回类型（并不是void），但是要求必须要return
	{
		cout << "operator int" << endl;
		return m_a;
	}
	operator int* ()
	{
		cout << "operator * " << endl;
		return &m_a;
	}
    // 同等条件下，重载类型和重载操作符都能匹配的情况下，重载操作符的优先级高
	int operator+(int a)
	{
		cout << "operator +" << endl;
		return m_a + a;
	}
};
int main(){
    CTest tst;
    int a = 0;
    a = tst;// operator int
    cout << a << endl;// operator+
    return 0;
}
```



## 第八章：list-map的简单使用

### list链表

```c++
#include<list>
using namespace std;
list<int>lst;
lst.push_front(1);//头插
lst.push_back(2);//尾插
lst.push_back(3);
lst.push_back(4);
lst.pop_front();//头删
lst.pop_back();//尾删
//---------------------------------
//迭代器遍历
list<int>::itreator ite=lst.begin();
while(ite!=lst.end())
{
    cout<<*ite<<" ";
    ite++;
}
cout<<endl;
//增强的范围for,若想修改链表内的值，可以在v前面加引用
for(int v:lst)
{
    cout<<v<<" ";
}
cout<<endl;
//-----------------------------------
//插入删除
list<int>::itreator ite1;
//在ite指向的位置之前插入value元素
ite1=lst.insert(ite,value);
//删除ite所在位置的元素，且自带++效果，返回的是删除位置的下一个结点的迭代器
ite=ite.erase(ite);//删除某个位置上的节点，参数中的迭代器会失效，一般用迭代器接一下返回值
lst.erase(ite++);
//-----------------------------------
//取首尾节点的值
lst.front();//*lst.begin()
lst.back();//*(--lst.end())
//-----------------------------------
//其他常见的函数
lst.empty();
lst.size();
lst.clear();
```

### map映射表

map为映射表，每一个元素被称为键值对，分为键值（key）和实值（value），键值唯一，所有元素以键值为标准排序

```c++
#include<map>
using namespace std;
	map<char,int> mm;
	mm['A'] = 4;
	mm['B'] = 6;
	mm.insert(pair<char,int>('E',5));//插入元素
	//迭代器遍历map
	map<char,int>::iterator ite = mm.begin();
	while(ite != mm.end())
	{
        // first: pair中的键值;second: pair中的实值
	    cout << ite->first << " " << ite->second <<endl;
	}
	//增强的范围for遍历map
	for(pair<char, int> pr: mm){
        cout << pr.first << "-" << pr.second << endl;
    }
//-------------------------------------
	pair<map<char, int>::iterator, bool>pr = mm.insert(pair<char, int>('a', 5));//插入成功，返回true，迭代器为新增的元素
	if (pr.second)
	{
		cout << "插入成功" << endl;
	}
	else
	{
		cout << "插入失败" << endl;
	}
	cout << pr.first->first << "---" << pr.first->second << endl;
	//插入失败，返回false，迭代器指向已存在的元素
	pr = mm.insert(pair<char, int>('a', 6));//插入成功，返回true，迭代器为新增的元素
 	//如果键值存在，则插入失败，可通过bool判断，迭代器返回键值已存在的元素
//-------------------------------------
	map<char, int>::iterator ite = mm.upper_bound('b');//返回大于
	cout << ite->first << "-" << ite->second << endl;
	ite = mm.lower_bound('b');                         //返回大于等于
	cout << ite->first << "-" << ite->second << endl;
```

## 第九章：拷贝构造

### 转换构造：

**条件：**只有在一个参数，或者其他参数在构造函数中存在默认值时，才能发生转换构造

```c++
class CTest {
public:
	int m_a;
	CTest():m_a(0){}
	CTest(int a):m_a(a){   //转换构造：可以发生一个隐式类型转换
		cout << __FUNCTION__ << endl;
	}
    CTest(int a, int b = 0):m_a(a){  //转换构造：可以发生一个隐式类型转换
		cout << __FUNCTION__ << endl;
	}
    explicit CTest(int a, int b = 2):m_a(a){  //禁止发生隐式类型转换
		cout << __FUNCTION__ << endl;
	}
};
//定义对象
CTest tst3 = 1;
```

### 拷贝构造：

#### 拷贝构造函数：

```c++
CTest tst;
CTest tst2(tst);
//函数体代码：编译器提供的，函数体代码不为空，形参中对象中的成员依次给this对象中的成员做初始化操作
//一旦手动重构这个拷贝构造函数，编译器就不会提供默认的了
CTest(const CTest &tst):m_a(tst.m_a){}
```

用一个对象给另一个对象初始化，空类中会默认提供拷贝构造，一旦手动重构这个拷贝构造函数，编译器就不会提供默认的了

#### 浅拷贝问题：

从拷贝构造函数入手，编译器提供的默认拷贝构造函数是浅拷贝，所谓浅拷贝，就是指针这样的对象只会针对地址拷贝，而不是内存空间的拷贝，如果是深拷贝，那么地址指向的空间也会复制一份

1. 多个对象中的指针指向了同一个申请的空间（new），在回收对象时，同一块空间被释放多次，导致程序异常
2. 其中一个对象通过指针修改了指向空间里的值，那么其他对象使用的是修改之后的值

```c++
class CTest{
public:
    int m_a;
    int* m_p;
    CTest(int a):m_a(a), m_p(new int(1)){}
    ~CTest(){
        if(m_p) delete m_p;
        m_p = nullptr;
    }
    CTest(const CTest &t):m_a(t.m_a), m_p(t.m_p){
        cout << __FUNCTION__ << endl;
    }
};
```



#### 深拷贝使用：

```c++
//深拷贝：需要手动重构构造函数，为当前对象的指针单独开辟一块属于自己的空间，并将值也拷贝过来
	CTest(const CTest &tst):m_a(tst.m_a),m_p(tst.m_p){
		cout << __FUNCTION__ << endl;
		if (tst.m_p)
		{
			m_p = new int(*tst.m_p);
		}
	}
	~CTest()
	{
		if (m_p) delete m_p;
		m_p = nullptr;
	}
```

#### 不用值传递，使用引用或指针：

假设我们拷贝构造函数没有实现深拷贝，或者我们根本就没有写拷贝构造函数，然后创建一个函数，函数参数传递一个类的对象，那么再调用的时候就相当于将我们已有的对象拷贝给形参，那就会出现浅拷贝的问题，所以对于函数的参数，我们一般都用引用传递或指针传递

不过要是我们做了一个深拷贝，且不考虑空间，那用值传递也不会有错误，只是不推荐

#### operator=

默认的重载函数有默认的参数，不能把指针赋值给对象

```c++
class CTest
{
public:
	int m_a;
	int* m_p;
	CTest(int a):m_a(a),m_p(new int (3)){}
	//在空类中，默认会提供下面函数，参数为const当前类对象的引用（与拷贝构造相同），返回类型也为当前类对象的引用
	//编译器默认提供的这个函数体代码，形参中对象中的成员依次给this对象中的成员做赋值操作
	//手动提供后，编译器不提供默认的
	//CTest& operator=(const CTest& tst)
	//{
	//	this->m_a = tst.m_a;
	// return *this;
	//}
	//默认提供的operator=是浅拷贝，会有浅拷贝的问题
	//解决 手动实现深拷贝
	CTest &operator=(const CTest& tst)
	{
		if (this != &tst)// 判断是否是自己
		{
			this->m_a = tst.m_a;
			//this->m_p = tst.m_p;
			if (tst.m_p)//不为空
			{
				if (this->m_p)
				{
					*this->m_p = *tst.m_p;
				}
				else
				{
					this->m_p = new int(*tst.m_p);
				}
			}
			else//为空
			{
				if (this->m_p)//将this指针释放掉
				{
					delete this->m_p;
					this->m_p = nullptr;
				}
			}
		}	
		return *this;
	}
	~CTest()
	{
		if (m_p) delete m_p;
		m_p = nullptr;
	}
};
```

### 空类中有哪些默认的函数

1. 默认无参的构造函数
2. 析构函数
3. 拷贝构造函数
4. 默认的重载等号操作符（参数为当前类对象）

## 第十章：设计模式

### 单例模式（Singleton Pattern）：

#### 属于创建型模式：

1. 当前的类最多只能创建一个对象（实例）
2. 这个唯一的实例，由当前类创建
3. 需要提供公共的接口，在整个系统中都可以访问。

#### 优点：

1. 单例模式提供了严格的对唯一实例的创建、访问和销毁，安全性高。
2. 单例模式的实现可以节省系统资源。

#### 懒汉式：现用现创建

```c++
#include<iostream>
using namespace std;
//懒汉式：当第一次调用这个接口函数时，现创建单例，时间换空间
//饿汉式：无论是否调用获取单例接口函数，都会提前创建单例，空间换时间
class CSingleton
{
	CSingleton(){}
	CSingleton(const CSingleton&) = delete;//禁用拷贝构造
	static CSingleton* m_pSing;
	~CSingleton() {}//把析构变成私有的，防止在类外delete psin1，防止m_pSing无法被回收
public:
	static class DeleteSingleton
	{
	public:
		~DeleteSingleton()
		{
			if (m_pSing)
				delete m_pSing;
			m_pSing = nullptr;

		}
	}del;
public:
	//有问题的代码，在多线程下可能会创建出多个对象
	static CSingleton* GetSingleton()
	{
		//加锁
		if (!m_pSing)
			m_pSing = new CSingleton;//如果指针为空，则创建对象
		//解锁
		return m_pSing;
	}
	//~CSingleton()
	//{
	//	if (m_pSing)
	//		delete m_pSing;
	//	m_pSing = nullptr;
	//}
	static void DestorySingleton(CSingleton*& psin)//引用
	{
		if (m_pSing)
			delete m_pSing;
		m_pSing = nullptr;
		psin = nullptr;
	}
};
CSingleton::DeleteSingleton CSingleton::del;//定义，静态的在程序结束自动回收，调用析构回收m_pSIng
CSingleton* CSingleton::m_pSing(nullptr);//类外定义初始化

int main()
{
	CSingleton* pSin1 = CSingleton::GetSingleton();
	CSingleton* pSin2 = CSingleton::GetSingleton();
	cout << pSin1 << " " << pSin2 << endl;//地址相同
	//-------------------------------------------
	//CSingleton sin2(*pSin1);//拷贝构造
	CSingleton::DestorySingleton(pSin1);
	if (pSin1)
	{
		cout << "---" << endl;
		*pSin1;
	}
	else
	{
		cout << "空" << endl;
	}
	return 0;
}
```

#### 饿汉式：提前创建

```c++
#include<iostream>
using namespace std;
class CSingleton
{
	CSingleton() {}
	CSingleton(const CSingleton&) = delete;//禁用拷贝构造
	static CSingleton sing;
	~CSingleton(){}//防止在类外delete psin1
public:
	static CSingleton* GetSingleton()
	{
		return &sing;
	}
};
CSingleton CSingleton::sing;
int main()
{
	CSingleton* pSin1 = CSingleton::GetSingleton();
	CSingleton* pSin2 = CSingleton::GetSingleton();
	cout << pSin1 << " " << pSin2 << endl;//地址相同
	return 0;
}
```

### 工厂模式

#### 简单工厂

```c++
#define _CRT_SECURE_NO_WARNINGS 1
#include <iostream>
using namespace std;
//基类
class CEngine {
public:
	virtual void working() = 0;
};

class CEngine2L: public CEngine {
public:
	void working() {
		cout << "2.0自然吸气发动机正在工作" << endl;
	}
};
class CEngine2T:public CEngine {
public:
	void working() {
		cout << "2.0涡轮增压发动机正在工作" << endl;
	}
};
class CFactoryEngine {
public:
	CEngine* CreateEngine(const string& type) {
		if (type == "2.0L") {
			return new CEngine2L;
		}
		else if (type == "2.0T") {
			return new CEngine2T;
		}
		else {
			return nullptr;
		}
	}
};
class CCar {
public:
	CEngine* m_pEngine;
	CCar() :m_pEngine(new CEngine2L) {}
	CCar(const string& type) :m_pEngine(type == "2.0L" ? (CEngine*)new CEngine2L : (CEngine*)new CEngine2T) {};
	// 引入工厂后的写法
	CCar(CFactoryEngine* pFac, const string& type) :m_pEngine(pFac ? pFac->CreateEngine(type) : nullptr) {};
	void drive() {
		if (m_pEngine) {
			m_pEngine->working();
			cout << "我的小汽车在呜呜跑着" << endl;
		}
	}
	~CCar() {
		if (m_pEngine) {
			delete m_pEngine;	
		}
		m_pEngine = nullptr;
	}
};
int main() {
	CFactoryEngine fac;
	CCar audi(&fac, "2.0L");
	audi.drive();
	CCar benz(&fac, "2.0T");
	benz.drive();
	return 0;
}
```

## 第十一章：模板

### 函数模板

#### 常规使用

```c++
//template:定义模板的关键字
//typename:定义模板类型的关键字
//<>:指定模板的函数列表
//模板函数，在调用函数时，模板类型可以通过实参自动推导（前提：在形参中使用了模板类型）
template<typename T>
T fun(T a, T b){
	return a + b;
}

int main(){
	int a = 10, b = 20;
	fun(a, b);// 其中T自动推导为int类型
	return 0;
}
```

#### 显示指定及默认值

```c++
template<typename T>
void fun(){
    T t = 0;
    cout << typeid(t).name() << endl;
}

template<typename T = int>
void fun2(){
    T t = 0;
    cout << typeid(t).name() << endl;
}

template<typename T = long>
void fun3(T t){
	cout << typeif(t).name() << endl;
}

int main(){
    fun<double>(); // 调用函数时，显式指定模板类型
    fun<short>(); // short
    fun2(); // int，使用默认的模板类型
    fun3<char>(48);// char
    fun3<>(48);// int
    //显式指定 > 实参自动推导 > 默认类型
    return 0;
}
```

##### 确认模板类型：

1. 函数如果带有形参且在形参中使用了模板，则可以通过实参自动推导（在函数调用时确定）
2. 函数模板可以指定默认的类型
3. 在调用函数时，可以显式指定默认模板类型

**显式指定 > 实参自动推导 > 默认类型**



#### 
