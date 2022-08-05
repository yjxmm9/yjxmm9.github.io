# C++核心编程



## 1 内存分区模型

代码区、全局区、栈区、堆区



### 1.1程序运行前

代码区：

​	存放CPU执行的机器指令

​	代码区是**共享**的，多次执行也只执行内存中的同一段代码

​	代码区是**只读**的，防止意外修改

全局区：

​	包含**全局变量**，**静态变量**和常量

​	常量区包括字符串常量和全局常量

​	该区域数据在程序结束后由操作系统释放



### 1.2 程序运行后

栈区：

​	由编译器自动分配释放

​	存放局部变量，函数的参数值

​	不要返回局部变量的地址

堆区：

​	由程序员分配释放

​	若程序员不释放，程序结束时由操作系统回收

​	使用new关键字在堆区开辟内存，用指针接收



### 1.3 new操作符

​	C++中利用new操作符在堆区开辟数据

​	用delete关键字手动释放空间

​	new创建的数据用指针接收

```c++
int* a=new int(10);//整数10
delete a;
int* arr=new int[10];//长度为10的数组
delete[] arr;//释放数组
```







## 2 引用



### 2.1 引用基本使用

​	作用：给变量起别名

​	语法：`int a=10; &b=a;`

​				数据类型 &别名 = 原名;



### 2.2 引用使用注意事项

​	引用必须初始化：`int &a;//错误`

​	引用初始化后不能更改：`int &b=a; &b=c;//错误`



### 2.3 引用做函数参数

​	作用：函数传参时，可以利用引用让形参修改实参

​	优点：简化了指正修改实参的操作

```c++
void Swap(int &a,int &b)
{
	int temp=a; a=b; b=temp;
}
int main()
{
	int a=10,b=20;
	Swap(a,b);//可以成功转换位置
}
```



### 2.4 引用作为函数返回值

​	不要返回局部变量的引用

```c++
int& test01()
{
    int a=10;
    return a;
}

int main()
{
    int &ref=test01();//错误
}
```

​	函数调用可以作为左值

```c++
int& test02()
{
    static int a=10;
    return a;
}

int main()
{
    int &ref=test02();
    cout<<ref<<endl;//输出10
    test02()=1000;//函数调用作为左值
    cout<<ref<<endl;//输出1000
}
```



### 2.5 引用的本质

​	本质：引用的本质就是指针常量(可以修改指向的内存值，但不能修改指向的内存位置)

```c++
int main()
{
	int a=10;
	int &ref=a;//等价于int* const ref=&a;
}
```



### 2.6 常量引用

​	作用：引用作为形参时前面加`const`关键字，用于修饰形参，防止形参改变实参

```c++
void showValue(const int &a)
{
    a=1000//会报错，因为形参是常量引用，防止了误操作
    cout<<a<<endl;
}

int main()
{
    //引用不能直接赋值
    int &ref=10;//错误
    const int &ref=10;//正确，编译器优化代码，int temp=10; const int &ref=temp;
    
    int a=10;
    showValue(a);
}
```





## 3 函数提高



### 3.1 函数默认参数

​	C++中，函数的形参列表是可以设置默认值的

​	**如果某个位置的参数有默认值，那他后面的所有参数都要有默认值**

​	**函数的声明和实现只能有一个有默认值**

```c++
int fun01(int a,int b,int c);//函数的声明

int fun01(int a，int b=10,int c=20)//函数的实现
{
   return a+b+c; 
}

int main()
{
    fun01(20,20)//输出20+20+20=60
}
```



### 3.2 函数占位参数

​	C++中，函数的参数列表可以有占位参数用来占位，调用函数时需要填补该位置

​	占位参数也可以有默认值,有默认值的占位参数的位置在调用时可以不用写参数具体值

```c++
void fun01(int a,int,int=10)//第二个参数为占位参数
{
    //省略内容
}

int main()
{
    fun01(10,10);//合法调用
    fun01(10);//不合法调用,第二个位置必须有参数
}
```

​	具体应用：[4.5.3 递增运算符的重载](#4.5.3 递增运算符重载)



### ==3.3 函数重载==

#### 	3.3.1 函数重载的概念

​		相同函数名有不同作用，提高复用性

​		函数重载需要满足的条件：

​				1）同一作用域下

​				2）相同函数名

​				3）函数参数的个数，类型或顺序不同

​		注意：返回值不同不能作为函数重载的条件

```c++
void fun01()
{
    //省略内容
}

void fun01(int a)//fun01的一个重载
{
    //省略内容
}
```



#### 	==3.3.2 函数重载注意事项==

​		引用作为重载条件时，`const int &a和int &a`存在区别

```c++
void fun01(int &a)
{
    //省略内容
}

void fun01(const int &a)
{
    //省略内容
}

int main()
{
    int a=10;
    fun01(a);//调用第一个
    fun01(10);//调用第二个
}
```

​		函数重载不要使用默认参数

```c++
void fun02(int a)
{
    //省略内容
}

void fun02(int a,int b=10)
{
    //省略内容
}

int main()
{
    fun02(10);//产生二义性，两个都可以调用，编译器不知道调用哪个
}
```



## 4 类和对象



​	==**面向对象三大特性：封装，继承，多态**==



### 4.1 封装

​	`Class 类名{ 访问权限：属性/行为 }`

#### 	4.1.1 封装的意义

- 将属性和行为作为整体表现

- 将属性和行为加以权限控制：public private protected

#### 	4.1.2 struct 和 class区别

​		C++中，struct和class的==唯一区别是默认访问权限不同==

​		`struct`默认权限是public       `class`默认权限是private

#### 	4.1.3 成员属性设置为私有

​		优点1：可以自己控制读写权限

​		优点2：对于写权限我们可以检测有效性

```c++
Class Person
{
private:
	string m_name;
public:
    void getName()//读
    {
        return m_name
    }
    void setName(string name)//写
    {
        if()//控制有效性
        {
            
        }
        m_name=name;
    }
};
```



### 4.2 对象初始化和清理

#### 	4.2.1 构造函数和析构函数

​		两个都是必须有的实现，如果自己不提供，编译器会自动提供空实现的构造和析构

​		**构造函数：**

​			作用：用于在创建对象时给对象的成员属性赋值

​			语法：

​				1）没有返回值，不写void

​				2）函数名称和类名相同

​				3）构造函数可以有参数，因此可以发生重载

​				4）调用对象的时候会自动调用构造函数，无需手动调用，且只会调用一次

​			==注意：构造函数不能是虚函数，不能够被重写！==

​				原理：https://blog.csdn.net/weixin_40637594/article/details/108755804

​		**析构函数：**

​			作用：用于对象销毁前执行一些清理工作，如堆区开辟的数据做释放操作

​			语法：

​				1）没有返回值，不写void

​				2）函数名称和类名相同，名称前加~

​				3）析构函数不可以有参数

​				4）在对象销毁前会自动调用析构函数，无需手动调用，且只会调用一次

#### 	4.2.2 构造函数的分类和调用

​		两种分类：

​			按参数分：无参构造和有参构造

​			按类型分：普通构造和拷贝构造

```c++
class Person
{
    Person()//无参
    {
      
    }
    
	Person(int a)//有参
    {
        
    }
    
    Person(const Person &a)//拷贝
    {
        
    }
};
```

​		

​		三种调用方式：

​			1.括号法:

```c++
Person p1(10);
Person p1(p2);//拷贝
Person p1();//错误，无参构造不能加括号，编译器会误以为是一个函数
Person p1;//无参的正确调用方法
```

​			2.显示法：

```c++
Person p1=Person(10);
Person p1=Person(p2);//拷贝
Person(10);//单独写是一个匿名对象，这行结束后立即析构
```

​			3.隐式法：

```c++
Person p1=10;
Person p1=p2;//拷贝
Person(p1);//错误，不能用拷贝构造初始化匿名对象，编译器会认为Person(p1)==Person p1;
```



#### 	==4.2.3 拷贝构造函数调用时机==

​		拷贝构造函数调用时机有三种

​		1）使用一个已经创建的对象去初始化新对象

​		2）值传递的方式给函数参数传值，会自动调用拷贝构造函数

​		3）函数return局部对象



#### 	4.2.4 构造函数调用规则

​		C++编译器会默认给类添加三个函数：

​		1）默认构造函数（无参，空）

​		2）默认析构函数（无参，空）

​		3）默认拷贝函数，对属性进行值拷贝

​		有以下规则：

​		1）如果定义了有参的构造函数，就不会默认提供默认无参构造，但会提供默认拷贝构造

​		2）如果定义了拷贝构造，就不会再提供其他构造函数



#### 	==4.2.5 深拷贝和浅拷贝==

​		浅拷贝：简单的赋值拷贝操作，如默认的拷贝构造函数就是浅拷贝

​		深拷贝：在**堆区域内**重新申请空间，进行拷贝操作

```c++
class Person{
    public:
    
    Person()//构造
    {
        m_height=new int(160);//在堆上申请空间
    }
    
    Person(const Person& p)
    {
        m_height=new int(*p.m_height);//给拷贝后新的对象的height申请堆空间
    }
    
    ~Person()
    {
        if(m_height!=NULL)//手动销毁堆区域
        {
            delete m_height;
            m_height=NULL;
        }
    }
    
    private:
    int* m_height;
};

int main()
{
    Person p1;
    Person p2(p1);//深拷贝
}
```



#### 	4.2.6 初始化列表

​		作用：用来初始化属性

```c++
class Person{
    public:
    
    int m_a,m_b,m_c;
    
    Person():m_a(10),m_b(20),m_c(30)//第一种
    {
        
    }
    
    Person(int a,int b,int c):m_a(a),m_b(b),m_c(c)//第二种
    {
        
    }
};
```



#### 	4.2.7 类对象作为类成员

```c++
class A{
    public:
    A(string s){
        m_s=s;
    }
    
    string m_s;
};
class B
{
    public:
    
    B(string s):a(s)//隐式转换法 A a=s;
    {
        
    }
    
    private:
    A a;
};
```

​		构造函数调用顺序:先构造成员中的类再构造该类

​		析构函数调用顺序和构造函数相反



#### 	4.2.8 静态成员

​		使用static关键字

​		访问方式：

​			1）通过对象：`Person p; p.a; p.func();`

​			2）通过类名：`Person::a; Person::func();`

​		静态成员分为：

​		一、静态成员变量

​			特点：

​				1）所有对象公用一份数据

​				2）编译阶段分配内存

​				==3）类内声明，类外初始化==

```c++
class Person{
    public:
    static int a;//类内声明
};
int Person::a=10;//类外初始化
```

​			二、静态成员函数

​				特点：

​					1）所有对象共享同一个函数

​					2）只能访问静态成员变量



### 4.3  C++对象模型和this指针





#### 	4.3.1 成员变量和成员函数分开存储

​		C++中，类内的成员变量和成员函数分开存储

​		只有非静态成员变量才属于类的对象，占类的空间

​		空对象的sizeof为1



#### 	4.3.2 this指针的用途

​		this指针是成员函数的隐含参数，指向调用的对象

​		1.当形参和成员变量同名时，用this来区分

​		2.在类的非静态成员函数中返回对象本身，可使用`return *this`



#### 	4.3.3 空指针访问成员函数

​		C++中空指针也能调用成员函数，但是要注意有没有用this

​		如果用到this指针，可能会报错

```c++
class Person{
  public:
    void showPerson()
    {
        //内容不能出现this指针
    }
    
    void showAge()
    {
        cout<<age<<endl;
    }
    
    int age=10;
};

int main()
{
    Person *p=NULL;
    p.showPerson();//合法。空指针可以调用成员函数
    p.showAge();//报错，第10行age使用到了this（编译器默认为this->age）
}
```

​	

#### 	4.3.4 const修饰成员函数 

​		常函数：

​			成员函数后加const

​			==常函数内不可以修改成员属性==

​			成员属性声明时加mutable，在常函数中就能修改了

​		常对象：

​			声明对象前加const

​			==常对象只能调用常函数==

```c++
class Person
{
    public:
    
    //this指针的本质 是指针常量 指针的指向是不能修改的
    //const Person *const this;
    //在成员函数后面加const，修饰的是this，让指针指向的值也不能发生改变
    void showPerson() const//常函数
    {
        m_a=10;//报错
        m_b=100;//合法,加了mutable
    }
    
    void func()
    {
        
    }
    
    int m_a;
    mutable int m_b;
};

int main()
{
    const Person p;//常对象
    p.m_a=100;//错误
    p.m_b=100;//合法
    p.func();//非法
    p.showPerson();//合法
```

​		

### 4.4 友元

作用：让一个函数或者类 访问另一个类中的私有成员

关键字：`friend`

友元的三种实现：

​	1）全局函数做友元

​	2）类做友元

​	3）成员函数做友元

#### 	4.4.1 全局函数做友元

```c++
class Building
{
    friend void func(Building *building);//全局函数作为友元
    
    public:
    Building()
    {
        this->bed_room="bedroom";
    }
    
    private:
    string bed_room;
};

void func(Building *building)
{
    cout<<building->bed_room<<endl;//可以访问了
}

int main()
{
    Building building;
    func(building);
}
```



#### 	4.4.2 类做友元

```c++
class Building{
    friend class GoodGay;//类作为友元
  
    public:
      Building()
      {
          this->bed_room="bedroom";
      }

     private:
     	string bed_room; 
};

class GoodGay{
    public:
    GoodGay()
    {
        building=new Building
    }
    
    void visit()
    {
        cout<<building->bed_room<<endl;//可以访问到bed_room
    }
    
    private:
    Building *building;
};

int main()
{
    GoodGay gg;
    gg.visit();
}
```

#### 	4.4.3 成员函数做友元

```c++
class GoodGay
{
  public:
    GoodGay()
    {
        building=new Building;
    }
    
    Building *building;
    
    void visit()
    {
        cout<<building->bed_room<<endl;//可以访问到bed_room
    }
};

class Buildimg
{
    friend void GoodGay::visit();//成员函数作为友元
    public:
    Building()
    {
        this->bed_room="bedroom";
    }
    private:
    string bed_room;
}

int main()
{
    GoodGay gg;
    gg.visit();
}
```



### 4.5 运算符重载

​	作用：对已有的运算符赋予新的意义，以适应不同的数据类型

#### 	4.5.1 加号运算符重载

​	作用：实现两个自定义数据类型相加的运算

```c++
class Person
{
    public:
    int m_a;
    int m_b;
    
    Person()
    {
        this->m_a=10;
        this->m_b=20;
    }
    
    Person operator+(Person &p)//成员函数重载+号运算符
    {
        Person temp;
        temp.m_a=this->m_a+p.m_a;
        temp.m_b=this->m_b+p.m_b;
        return temp;
    }
};

Person operator+(Person &p1,Person &p2)//全局函数重载+号运算符
    {
        Person temp;
        temp.m_a=p1.m_a+p2.m_a;
        temp.m_b=p1.m_b+p2.m_b;
        return temp;
    }

Person operator+(Person &p1,int &a)//运算符重载也可以有重载
    {
        Person temp;
        temp.m_a=p1.m_a+a;
        temp.m_b=p1.m_b+a;
        return temp;
    }

int main()
{
    Person p1,p2,p3;
    p3=p1+p2;//简化的调用重载+号函数
    p3=p1.operator+(p2);//成员函数的调用
    p3=operator+(p1,p2);//全局函数的调用
}
```

​	

#### 	4.5.2 左移运算符重载

​		作用：可以输出自定义的类型

​		不能用成员函数重载<<，只能用全局函数

```c++
class Person
{
    friend ostream &operator<<(ostream &cout,Person &p);
    
    private:
    int m_a=10;
    int m_b=20;
    
};

ostream &operator<<(ostream &cout,Person &p)//返回值是ostream可以实现链式编程的思想
{
    cout<<p.m_a<<p.m_b;
    return cout
}

int main()
{
    Person p;
    cout<<p<<"111"<endl;
}

```

​	

#### 	4.5.3 递增运算符重载

```c++
class myInt
{
    friend ostream &operator<<(ostream &cout,myInt i);
    
    public:
    myInt()
    {
        num=0;
    }
    
    myInt& operator++()//重载前置++,要返回引用
    {
        this->num++;
        return *this
    }
    
    myInt operator++(int)//重载后置++，要返回值，需要用占位参数
    {
        myInt temp=*this;
        this->num++;
        return temp;
    }
  
    private:
    int num;
};

ostream &operator<<(ostream &cout,myInt i)//重载<<
{
    cout<<i.num;
    return cout
}

int main()
{
    myInt a;
    cout<<a++<<endl;
    cout<<++a<<endl;
}
```



#### 	4.5.4 赋值运算符重载

​		C++编译器至少提供给一个类4个函数

​		1）2）3）构造，析构，拷贝构造

​		4）赋值运算符 operator=,对属性值进行拷贝

​		赋值运算符重载的作用：将浅拷贝修改为深拷贝，防止堆区域的数据发生错误

```c++
class Person
{
    public:
    
    Person(int age)
    {
       m_Age=new int(age);
    }
    
    int *m_Age;
    
    Person& operator=(Person &p)
    {
        if(this->m_Age!=NULL)
        {
            delete this->m_Age;
            this->m_Age=NULL;
        }
        m_Age=new int(*p.m_Age);//用深拷贝解决问题
    }
};

int main()
{
    Person p1(18);
    Person p2(20);
    Person p3(30);
    p3=p2=p1;
}
```



#### 	4.5.5 关系运算符重载

```c++
class Person
{
    public:
    
    int m_age;
    string m_name;
    
    Person(int age,string name)
    {
        m_age=age;
        m_name=name;
    }
    
    bool operator==(Person &p)
    {
        if(this->m_age==p.m_age&&this->m_name==p.m_name)
            return true;
        return false;
    }
    
        bool operator!=(Person &p)
    {
        if(this->m_age==p.m_age&&this->m_name==p.m_name)
            return false;
        return true;
    }
};

int main()
{
    Person p1(18,"Tom");
    Person p2(19,"Jerry");
    if(p1==p2){
        //省略内容
    }
    if(p1!=p2)
    {
        //省略内容
    }
}
```



#### 	4.5.6 函数调用运算符重载

​		函数调用运算符()也可以重载

​		重载的方式和函数调用比较像，所以又称为**仿函数**

​		仿函数写法灵活

```c++
class myPrint{
  public:
    void operator()(string text)
    {
        cout<<text<<endl;
    }
};

class myAdd
{
  public:
    int operator()(int a, int b)//仿函数写法灵活，输出值和参数可以根据情况调整
    {
        return a+b;
    }
};

int main()
{
    myPrint myprint;
    myprint("hello");//调用重载的()操作符
    
    cout<<myAdd()(10,10)<<endl;//匿名对象myAdd()调用（myAdd()这个整体是一个匿名对象）
}
```



### 4.6 继承

#### 	4.6.1 继承的基本语法

​		class 子类：继承方式   父类

​		`class A: public BaseClass`



#### 	4.6.2 继承方式

​		三种：`public private protected`

​		![](C:\Users\qingl\Downloads\微信截图_20220516122525.png)

​		

​		父类private任何继承方式子类都无法访问

​		公有继承父类成员访问权限在子类里不变

​		保护继承父类的public，protected的成员在子类里都变成protected

​		私有继承父类的public，protected的成员在子类里都变成private



#### 	4.6.3 继承中的对象模型

​		父类中所有非静态成员属性都会被子类继承下去

​		子类sizeof==父类非静态成员属性sizeof + 子类非静态成员属性szieof；

​		父类中私有成员属性 被编译器隐藏了 虽然访问不到 但确实被继承了



#### 	4.6.4 继承中构造和析构顺序

​		先构造父类，再构造子类；析构顺序和构造顺序相反



#### 	4.6.5 继承**同名**成员处理方式

​		==子类会隐藏父类同名成员函数==		

​		子类对象加作用域才能访问到父类**同名**成员

​		子类和父类有同名成员函数时，子类会隐藏父类中**同名**成员函数，加作用域才可以访问父类中**同名**成员函数

```c++
class Base
{
    public:
    void A()
    {
        
    }
    
    void A(int a)
    {
        
    }
    
    int m_a;
};

class Son:public Base
{
    public:
    void A()
    {
        
    }
    
    int m_a;
}

int main()
{
    Son son;
    cout<<son.Base::m_a<<endl;//调用父类同名成员属性m_a
    son.A(10);//报错！子类会隐藏父类同名成员函数
    son.Base::A();//调用父类同名成员函数
}
```



#### 	4.6.6 继承同名静态成员处理方式

​		处理方式和非静态成员同名的处理方式相似

```c++
class Base
{
    public:
    static void A()
    {
        
    }
    
     static int m_a;
};
int Base::m_a=10;

class Son:public Base
{
    public:
    static void A()
    {
        
    }
    
     static int m_a;
};
int Son::m_a=20;

int main()
{
    Son son;
    //1.用对象访问
    cout<<son.Base::m_a<<endl;
    son.Baes::A();
    
    //2.用类名访问
    cout<<Base::m_a<<endl;
    //如果一定要从son到base访问
    cout<<Son::Base::m_a<<endl;
    Base::A();
    Son::Base::A();
}
```



#### 	4.6.7 多继承

​		==C++实际开发中不建议多继承==

​		多继承可能会引发父类中有同名成员出现，需要加作用域区分

```c++
class Base1
{
    public:
    int m_a=5;
};

class Base2
{
    public:
    int m_a=10;
};

class Son:public Base1,public Base2//多继承语法
{
    public:
    int m_a=15;
}

int main()
{
    Son son;
    //多继承可能会引发父类中有同名成员出现，需要加作用域区分
    son.Base1::m_a=11;
    son.Base2::m_a=11;
}
```

​		

#### 	4.6.8 虚继承解决菱形继承

​		菱形继承的概念：

​			两个派生类继承同一个类

​			又有另一个类同时继承这两个派生类

​			会产生意义相同的一个数据有两份继承到多继承的子类中

![](C:\Users\qingl\Downloads\微信截图_20220516143802.png)

​		虚继承：`class Son:virtual public Base{};`

​		虚继承的原理：

​			当我们在继承方式前加virtual 关键字，进行虚继承，虚继承内存中会通过虚基类指针指向虚基类表，该表中的数据是为虚基类指针到基类的存储区域的偏移值。

![微信截图_20220516143712](C:\Users\qingl\Downloads\微信截图_20220516143712.png)

```c++
class Animal
{
    int age;
};

class Sheep:virtual public Animal//虚继承Animal
{
    
};

class Tuo:virtual public Animal//虚继承Animal
{
    
};

class SheepTuo:public Sheep,public Tuo
{
    
};
int main()
{
    SheepTuo st;
    st.age=10;//不会产生Sheep.age和Tuo.age的冲突,SheepTuo下只有一个age
}
```



### ==4.7 多态==

#### 	==4.7.1 多态基本概念==

​		多态分为两类：

​			静态多态：函数重载，运算符重载

​			动态多态：派生类、**虚函数**实现运行时的多态

​		静态多态和动态多态的区别：

​			静态多态的函数地址**早绑定**——编译阶段确定函数地址

​			动态多态的函数地址**晚绑定**——运行阶段确定函数地址，函数前加`virtual`变成虚函数就可以实现晚绑定

```c++
class Animal{
    public:
    virtual void Speak()
    {
        
    }
};

class Cat:public Animal
{
    public:
    void Speak()
    {
        
    }
};

void doSpeak(Animal &animal)//Aniaml &animal=cat;
{
    animal.Speak();
}

int main()
{
    Cat cat;
    doSpeak(cat);//调用的是Cat的Speak()，如果Animal的Speak()不是virtual就只能调用Animal的Speak()
}
```

​		==重写概念：函数返回值 函数名 参数列表 完全一致成为重写（注意和重载区分！！）==		

​		多态满足条件：

​			1.有继承关系

​			2.子类**重写**父类中的虚函数

​		**多态使用：父类指针或引用指向子类对象**，如`Aniaml &animal=Cat cat;`

​		多态本质原理：父类虚函数的内部结构如图，此时**父类虚函数表内部是父类函数地址**

![微信截图_20220516154458](C:\Users\qingl\Downloads\微信截图_20220516154458.png)

​			子类继承父类时，若子类重写父类的函数后，子类虚函数表内部会被**替换成为子类虚函数地址**，子类内部结构如图：

![微信截图_20220516154814](C:\Users\qingl\Downloads\微信截图_20220516154814.png)

​				此时当父类指针或引用指向子类对象时，发生多态

​				`Animal &animal=Cat cat;//父类引用指向子类对象`

​				`Animal *animal=new Cat;//父类指针指向子类对象，开辟在堆区域，记得使用delete销毁`

![微信截图_20220516155232](C:\Users\qingl\Downloads\微信截图_20220516155232.png)



#### 	4.7.2 多态好处

​		1、组织结构清晰

​		2、可读性强

​		3、对于前期和后期扩展以及维护性高



#### 	4.7.3 纯虚函数和抽象类

​		在多态中，通常父类中虚函数的实现没有意义，主要都是用来被重写

​		因此可以将虚函数改为**纯虚函数**

​		纯虚函数语法：`virtual 返回值类型 函数名 (参数列表) = 0`

​		类中只要有一个纯虚函数，这个类就被称为**抽象类**

​		**抽象类特点：**

​			1.无法实例化对象（不能堆上实例化`new 抽象类`  或者 栈上实例化 `抽象类名  对象名`）

​			2.抽象类子类 必须重写父类的**纯虚函数** 否则也属于抽象类

​		**注意**：虚函数和纯虚函数的区别：虚函数在子类可以不进行重写，纯虚函数必须要在子类重写，否则无法调用



#### 	==4.7.4 虚析构和纯虚析构==

​		多态使用时，如果子类有属性开辟在堆区域，那么**父类指针释放时无法调用到子类的析构代码**

​		解决方法：使用虚析构或纯虚析构

​		虚析构和纯虚析构**共同点**：

​			可以解决父类指针释放子类对象

​			都需要具体函数的实现

​		虚析构和纯虚析构的**不同点**：

​			如果是纯虚析构，就是抽象类，不能实例化对象





## 5  文件操作

​	C++中对文件操作需要包含头文件`<fstream>`

​	文件类型分为两种：

​		1）文本文件——以ASCII码存储在计算机里

​		2）二进制文件——以二进制形式存储在计算机里

​	操作文件的三大类：

​		1.`ofstream`：写操作

​		2.`ifstream`：读操作

​		3.`fstream`：读写操作



### 5.1 文本文件

#### 	5.1.1 写文件

​		步骤如下：

​			1）包含头文件：`#include <fstream>`

​			2）创建流对象：`ofstream ofs;`

​			3）打开文件：`ofs.open("文件路径"，打开方式);`

​			4）写数据：`ofs<<"写入的数据"`

​			5）关闭文件：`ofs.close();`

​		文件打开方式：

![](C:\Users\qingl\Downloads\微信截图_20220516172621.png)

​			注意：文件打开方式可以配合使用，利用操作符`|`，如`ios::out|ios::binary`

​	

#### 		5.1.2 读文件

​			步骤如下：

​			1）包含头文件：`#include <fstream>`

​			2）创建流对象：`ifstream ifs;`

​			3）打开文件：`ifs.open("文件路径"，打开方式);`

​				利用`if(!ifs.isopen())`可以判断文件是否打开成功

​				判断文件是否为空：

```c++
char ch;
ifs>>ch;
ifs.eof()//判断读入的是否是文件的末尾EOF
```

​			4）读数据：四种

```c++
//第一种
char buf[1024]={0};
while(ifs>>buf)
{
    cout<<buf<<<endl;
}

//第二种
char buf[1024]={0};
while(ifs.getline(buf,sizeof(buf)))
{
    cout<<buf<<endl;
}

//第三种
string buf;
while(getline(ifs,buf))
{
    cout<<buf<endl;
}



//第四种（不推荐）
char c;
while( (c=ifs.get()) ! = EOF)//end od file
{
    cout<<c;
}

//按顺序读入定义的数据
int a,b;//要读入的数据
while(ifs>>a&&ifs>>b)
{
    //进行需要的操作，如num++等
}
```

​			5）关闭文件：`ifs.close();`



### 5.2 二进制文件

打开方式要指定为`ios::binary`

#### 	5.2.1 写文件

​		基本步骤和文本文件的写文件相似

​		第四步写文件

```c++
Person p;//需要写入的对象
ofs.write((const char*)&p,sizeof(Person));
```

注意：操作文件时**尽量不要用string**，容易报错

#### 	5.2.2 读文件

​		基本步骤和文本文件的读文件相似

​		第四步读文件

```c++
Person p;
ifs.read((char*)&p,sizeof(Person));
cout<<p.age<<p.name<<endl;
```

​		



## 6 补充部分

### 	6.1 输入输出流

​		遇到cin无法输入值的情况时，需要使用以下函数：

```c++
cin.clear();//更改cin的状态标识符
cin.ignore(1024,'\n');//清除输入缓冲区的所有内容，直到遇到回车符为止， 各种编译器都有效
```

​		具体详见：https://blog.csdn.net/qq_43152052/article/details/89323153
