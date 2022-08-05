# C++提高编程

注意：

==STL不同容器，算法的头文件以及遍历输入等为了方便进行了省略，请在实际应用时注意添加==

主要包括==泛型编程==和==STL==技术



## 1 模板

### 1.1 模板概念

​	模板的特点：

​		1、模板不能直接使用，她只是一个框架

​		2、模板的通用并不是万能的

​	C++的另一种==编程思想称为泛型编程==，主要利用模板技术

​	C++提供两种模板机制：==函数模板和类模板==

### 1.2 函数模板

#### 1.2.1 函数模板语法

​	函数模板作用：建立通用函数，函数的返回值类型和形参类型可以不确定，用虚拟类型代表

​	语法：

```c++
template<typename T>
//接函数声明或定义
```

​	`	template`——声明创建模板

​	`typename`——表面其后面的符号是一种数据类型，可以用`class`代替

​	`T`——通用的数据类型，可以替换，通常用T

```c++
template<typename T>//声明模板，告诉编译器后面的T不要报错
void Swap(T &a,T &b)
{
    T temp;
    temp=a;a=b;b=temp;
}

int main()
{
    int a=10,b=20;
    //两种调用方式
    //自动类型推导
    Swap(a,b);
    //显示指定类型
    Swap<int>(a,b);
}
```



#### 1.2.2 函数模板注意事项

​	1）自动类型推导，必须推出一致T才可以使用

​	2）模板必须要能够确定出T的具体数据类型，才可以使用



#### 1.2.3 普通函数和模板函数的区别

​	普通函数调用可以发生隐式类型转换

​	模板函数要用显示指定类型法去调用才能隐式类型转换，如果用自动类型推导则不能隐式类型转换，因为有歧义

```c++
int myAdd(int a,int b)//普通函数
{
    return a+b;
}

template<class T>
T myAdd(T a,T b)
{
    return a+b;
}

int main()
{
    int a=10;
    char c='c';
    //调用普通函数时
    myAdd(a,c);//可以运行，char字符会隐式类型转换成ascii码
    //自动推导类型调用时
    myAdd(a,c);//报错
    //显示指定类型调用时
    myAdd<int>(a,c);//合法，结果和普通函数运行结果相同
}
```



#### 1.2.4 普通函数和函数模板调用规则

​	1）若函数模板和普通函数都可以实现，优先调用普通函数，即使函数只有声明（会报错）

​	2）可以通过空模板参数列表（eg.`maAdd<>(a,b)`）来强制调用函数模板

​	3）函数模板也可以发生重载

​	4）若函数模板可以更好的匹配，优先调用函数模板



#### 1.2.5 模板的局限性

​	模板的通用性并不是万能的，对于一些自定义数据类型的，需要具体化的实现（如Person类是否相等的比较）

```c++
template<class T>
bool myCompare(T a,T b)//如果用自定义类调用这个模板函数是不行的
{
    if(a==b)
    {
        return true;
    }
    else
    {
        return false;
    }
}

template<> bool myCompare(Person &p1,Person &p2)//对于特殊类型的具体化实现
{
    //具体化的实现
    if(p1.age==p2.age&&p1.id==p2.id)
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

​	**学习模板不是为了写模板，而是在STL能够运用系统提供的模板**



### 1.3 类模板



#### 1.3.1 类模板语法

​	作用：建立通用类，类中成员类型可以不具体定制，用虚拟类型代表

```c++
template <class NameType,class AgeType>//多个不确定类型
class Person
{
public:
    Person(NmaeTypr name,AgeType age)
    {
        this->m_name=name;
        this->m_age=age;
    }
    
    NameType m_name;
    AgeType m_age;
}

int main()
{
    Person<string,int>p("aaa",1111);//调用
}
```



#### 1.3.2 类模板和函数模板区别

​	1）类模板没有自动类型推导的调用方式

​	2）类模板的模板参数列表可以有默认值

```c++
template <class NameType,class AgeType=int>//模板参数列表可以设置默认值
class Person
{
public:
    Person(NmaeTypr name,AgeType age)
    {
        this->m_name=name;
        this->m_age=age;
    }
    
    NameType m_name;
    AgeType m_age;
}

int main()
{
    Person<string>p("aaa",1111);//此处不写int同样合法
}
```



#### 1.3.3 类模板中成员函数创建时机

​	普通类中成员函数和类模板中的成员函数调用时机有区别：

​		普通类中的成员函数一开始就创建

​		模板类中的成员函数在调用时才创建，因为在调用前编译器不能确定具体的类型



#### 1.3.4 类模板对象做函数参数

​	类模板实例化出的对象 作为函数参数传递的方式

​	最常用：指定转入的类型

```c++
template <class NameType,class AgeType=int>//模板参数列表可以设置默认值
class Person
{
public:
    Person(NmaeTypr name,AgeType age)
    {
        this->m_name=name;
        this->m_age=age;
    }
    
    void showPerson()
    {
        //省略内容
    }
    NameType m_name;
    AgeType m_age;
}

//方法一、指定传入的类型(最常用！！)
void showInfo( Person<string,int> &p)
{
    p.showPerson();
}

//方法二、参数模板化
template<class T1,class T2>
void showInfo(Person<T1,T2> &p)
{
    p.showPerson();
}

//方法三、整个类模板化
template<class T>
void showInfo(T &p)
{
    p.showPerson();
}

int main()
{
    Person<string,int>p("qqq",11);
    showInfo(p);
}
```

​	补充：查看参数的具体类型：`typeid(T).name();`

#### 1.3.5 类模板与继承

​	子类继承的父类时模板类时，子类声明时必须要指定出父类中T的类型，否则会报错

​	如果想灵活指定父类中T的类型，子类也需要时类模板

```c++
template<class T>
class Base
{
public:
    T a;
}

class Son1:public Base<int>//必须指定出父类T的类型，不能灵活指定类型
{
    
}

template<class T1,class T2>
class Son2:public Base<T1>//子类也变成类模板，就可以灵活指定父类T的类型
{
public:
    T2 b;
}
```



#### 1.3.6  类模板成员函数类外实现

​	类模板中成员函数类外实现时，需要在作用域后加上模板参数列表

```c++
template <class T1,class T2>
class Person
{
public:
    Person(T1 name,T2 age)//类内声明
    T1 m_name;
    T2 m_age;
}

//成员函数类外实现
template<class T1,class T2>
Person<T1,T2>::Person(T1 name,T2 age)//交代函数作用域
{
    //具体实现
}
```



#### 1.3.7 类模板分文件编写

​	类模板中成员函数创建时机是调用阶段，导致分文件编写时链接不到

​	有两种方法解决：

​		1）直接#include .cpp源文件

​		2）将声明和实现写到同一个文件夹，并更改后缀名为.hpp==（常用解决方法）==

```c++
//以下是Person.hpp的代码
#pragma once
#include<iostream>
using namespace std;
#include<string>

template <class T1,class T2>
class Person
{
public:
    Person(T1 name,T2 age)
    T1 m_name;
    T2 m_age;
};


template<class T1,class T2>
Person<T1,T2>::Person(T1 name,T2 age)
{
    //具体实现
}

//在源文件中#include"Person.hpp"就可以调用类模板了
```



#### 1.3.8 类模板与友元

​	全局函数类内实现：直接在类内声明友元即可==（建议使用这种）==

​	全局函数类内声明类外实现：需要提前让编译器知道全局函数的存在

```c++
template<class T1,class T2>class Person//让编译器提前知道Person类存在

template<class T1,class T2>
void Printp(Person<T1,T2> &p)//2.全局函数做友元类外实现，需要让编译器提前知道类模板和全局函数模板存在
{
    //具体实现
}
    
template<class T1,class T2>
class Person
{
    friend void Printp(Person<T1,T2> &p)//1.全局函数作为友元类内实现
    {
        //具体实现
    }
    
    friend void Printp<>(Person<T1,T2> &p)//2.全局函数做友元类内声明，加空模板参数列表
};

```



## 2 STL初识



### 2.1 STL诞生

​	C++的面向对象和泛型编程的思想，目的是为了提升复用性

​	为了建立数据结构和算法的一套标准，诞生了STL



### 2.2 STL基本概念

​	STL(Standard Template Library,标准模板库)

​	STL几乎所有代码都采用模板类或模板函数



### 2.3 STL六大组件

​	==**容器，算法，迭代器，仿函数**，适配器，空间适配器==

​	适配器：一种用来修饰容器或仿函数或迭代器接口的东西

​	空间适配器：负责空间的配置与管理

​	（两个不会涉及到具体内容）



### 2.4 STL中容器、算法、迭代器

​	**容器**：就是将运用最广泛的一些数据结构实现出来

​	容器分为：**==序列式容器和关联式容器==**

​		序列式容器：每个元素均有固定的位置，强调值的排序

​		关联式容器：二叉树结构，各元素间没有严格得物理上得顺序关系



​	算法：**==分为质变算法和非质变算法==**

​		质变算法：运算过程中会更改区间内元素的内容，如拷贝，插入，删除等

​		非质变算法：运算过程中不会更改区间内元素的内容，如查找，计数，遍历等



​	迭代器：**==容器和算法间的粘合剂==**

​		提供一种方法，使能够依序访问某个容器中的各个元素，而又无需暴露该容器的内部表示方式

​		每个容器偶有专属的迭代器

​		迭代器使用类似指针，可以暂时理解为指针

​		迭代器种类：输入迭代器，输出迭代器，向前迭代器，**双向迭代器，随机访问迭代器**

​			常用的两种：

​				双向迭代器：读写操作，并能向前向后操作（++，--）

​				随机访问迭代器：读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器



### 2.5 容器算法迭代器初识

​	STL中最常用的容器为vector，可以理解为数组

#### 2.5.1 vector存放内置数据类型

​	容器：`vector`

​	算法：`for_each`

​	迭代器：`vector<int>::iterator`

```c++
#include<vector>//引入vector头文件
#include<algorithm>//引入算法头文件

void myPrint(int a)
{
    cout<<a<<endl;
}

int main()
{
    vector<int> v;//vector定义
    v.push_back(10);//vector尾插元素
    v.push_back(20);
    v.push_back(30);
    
    //创建迭代器
    vector<int>::iterator pBegin=v.Begin();//v.Begin()返回迭代器，指向容器的第一个数据
    vector<int>::iterator pEnd=v.End();//v.End()返回迭代器，指向容器最后一个元素的下一个位置
    
    //第一种遍历方式
    while(pBegin!=pEnd)
    {
        cout<<*pBegin<<endl;
        pBegin++;
    }
    
    //第二种遍历方式
    for(vector<int>::iterator it=v.Begin();it!=v.End();it++)
    {
        cout<<*it<<endl;
    }
    
    //第三种方式，使用STL提供的标准遍历算法，需要头文件<algorithm>
    for_each(v.Begin(),v.End(),myPrint)//不用加()
    
}
```



#### 2.5.2  Vector存放自定义数据类型

```c++
class Person
{
public:
    Person(string name,int age)
    {
        this->m_name=name;
        this->m_age=age;
    }
    string m_name;
    int m_age;
}

int main()
{
    vector<Person> v;
    Person p1("aaa",111);
    Person p1("baa",211);
    
    v.push_back(p1);
    v.push_back(p2);
    
    for(vector<Person>::iterator it=v.Begin();it!=v.End();it++)
    {
        cout<<(*it).m_name<<(*it).m_age<<endl;
    }
}
```



#### 2.5.3 Vector容器嵌套容器

```c++
int main()
{
    vector<vector<int>> v;
    
    vector<int> v1;
    vector<int> v2;
    vector<int> v3;
    
    for(i=0;i<4;i++)
    {
        v1.push_back(i+1);
        v2.push_back(i+2);
        v3.push_back(i+3);
    }
    
    for(vector<vector<int>>::iterator it=v.Begin();it!=v.End();it++)
    {
        for(vector<int>::iterator mit=(*it).Begin();mit!=(*it).End();mit++)
        {
            cout<<*vit<<" ";
        }
        cout<<endl;
    }
}
```



## 3  STL常用容器

### 3.1 String容器

#### 3.1.1 string基本概念

​	本质：string是C++风格的字符串，而string本质上是一个类

​	string和char *区别：

​		char *是一个指针

​		string是一个类，是一个char *的容器，类内封装了char *，管理这个字符串

​	特点：

​		string类内部封装了很多成员方法，如find,copy,delete,replace,insert

​		string管理char *所分配的内存，不用担心复制越界和取值越界等，由类内部负责

​	

#### 3.1.2 string构造函数

```c++
int main()
{
    string s1;//string()创建空字符串
    
    //string(const char* s)使用字符串初始化
    const char* str="asd";//"asd"就是一个字符数组const char*
    string s2(str);
    
    //string(const string &s)使用一个string对象初始化另一个string对象
    string s3(s2);
    
    //string(int n,char c)使用n个字符c初始化
    string(10,'a');
}
```



#### 3.1.3 string赋值操作

```c++
int main()
{
    //使用operator=(常用)
    string s1="hewaeae";
    
    string s2=s1;
    
    string s3='a';
    
    //使用assign()
    string s4;
    s4.assgin("ihdqoid");
    
    string s5;
    s5.assgin("sddqwd",5);//前5个字符
        
    string s6;
    s6.assgin(s5);//拷贝
    
    string s7;
    s7.assgin(10,'a');//几个几
}
```



#### 3.1.4 string字符串拼接

​	实现在字符串末尾拼接字符

```c++
int main()
{
    string s1="aaaa";
    //使用+=操作符
    s1+="weqeewe";
    
    s1+='w';
    
    string s2="weeeee";
    s1+=s2;
    
    //使用append()
    s1.append("asda");
    
    s1.append("saddd",4);//前4个字符
    
    s1.append(s2);
    
    s1.append(s2,0,3);//s2从第0个位置开始的后面3个
}
```

​	

#### 3.1.5 string 查找和替换

```c++
int main()
{
    string s1="abcdefg";
    
    //find(),返回位置
    int pos=s1.find("de");
    
    //rfind()从右往左找到第一个符合条件
    pos=s1.rfind("de");
    
    //替换，s1的第一个位置开始后面三个字符被换成"weqwewe"
    s1.replace(1,3,"weqwewe");
}
```



#### 3.1.6 string比较

​	字符串比较按照ASCII码进行对比

​	= 返回 0

​	> 返回 1

​	< 返回 -1

```c++
int main()
{
    string s1="qwer";
    string s2="qqqq";
    
    if(s1.compare(s2)==0)//compare()比较字符串
    {
        cout<<"相等"<<endl;
    }
}
```



#### 3.1.7 string字符存取

```c++
int main()
{
	string s1="qwer";
    
    //1.使用[]获取字符
    for(i=0;i<s1.size();i++)//size()可以获取字符串长度
    {
        cout<<s1[i]<<endl;
    }
    s1[1]='a';//可以通过[]修改字符
    
    //2.使用at()获取字符
    for(i=0;i<s1.size();i++)
    {
        cout<<s1.at(i)<<endl;
    }
    s1.at(1)='a';//可以通过at修改单个字符
}
```



#### 3.1.8 string插入和删除

```c++
int main()
{
    string s1="qqwe";
    //插入
    s1.insert(1,"qwe");//第一个位置插入"qwe"
    
    //删除
    s1.erase(1.3);//从第一个位置删除后面3个字符
}
```



#### 3.1.9 string子串

从字符串中获取想要的字符

```c++
int main()
{
    //提取用户名
    string email="qing@163.com";
    int pos=email.find("@");
    string subStr=email.substr(0,pos);//substr()返回从0开始后面pos个字符的子串
}
```



### 3.2 Vector容器

#### 3.2.1 vector基本概念

​	vector和数组非常相似，又叫**单端数组**

​	vector和普通数组的区别：

​		普通数组是静态空间，而vector可以**动态扩展**

​	动态扩展：

​		==并不是在原空间之后继续接新空间，而是寻找更大的内存空间，然后将原数据拷贝进新空间，释放原空间==

​	vector容器的迭代器是支持**随机访问的迭代器**



#### 3.2.2 vector构造函数

​	创建vector容器

```c++
int main()
{
    vector<int>v1;//默认无参构造（常用）
    
    vector<int>v2(v1.Begin(),v1.End());//通过区间方式进行构造，区间左闭右开
    
    vector<int>v3(10,100);//10个100
        
    vector<int>v4(v3);//拷贝构造函数（常用）
}
```



#### 3.2.3 vector赋值操作

```c++
int main()
{
    vector<int>v1;
    for(i=0;i<10;i++)
    {
        v1.push_back(i);
    }
    
    //等号操作符赋值
    vector<int>v2;
    v2=v1;
    
    //assign()区间数据拷贝赋值
    vector<int> v3;
    v3.assign(v1.Begin(),v1.End());//左闭右开
    
    //n个elem赋值
    vector<int>v4;
    v4.assign(10,100);//10个100
}
```



#### 3.2.4 vector容量和大小

```c++
int main()
{
    vector<int>v1;
    for(i=0;i<10;i++)
    {
        v1.push_back(i);
    }
    
    v1.empty();//返回容器是否为空
    v1.capacity();//返回容器的容量
    v1.size();//返回容器中元素个数
    v1.resize(4);//重新指定容器大小，比原来大会用0填充;比原来小就把后面的删除，但容器容量不会变
    v1.resize(11,100);//比原来大用100填充
}
```



#### 3.2.5 vector插入和删除

```c++
int main()
{
    vector<int> v1;
    //尾插
    v1.push_back(10);
    
    //尾删
    v1.pop_back();
    
    //插入,第一个参数是迭代器
    v1.insert(v1.Begin(),100);
    v1.insert(v1.Begin(),2,1);//插入2个1
    
    //删除
    v1.erase(v1.Begin(),v1.End());
    
    //清空
    v1.clear();
}
```



#### 3.2.6 vector数据存储

```c++
int main()
{
    vector<int>v1;
    int i;
    
    //[]访问单个元素
    v1[i];
    
    //at()访问单个元素
    v1.at(i);
    
    //访问第一个元素
    v1.front();
    
    //访问最后一个元素
    v1.back();
}
```



#### 3.2.7 vector互换容器

```c++
int main()
{
    vector<int>v1;
    for(i=0;i<10000;i++)
    {
        v1.push_back(i);
    }
    
    vector<int>v2;
    
    //交换
    v1.swap(v2);
    
    //巧用交换可以收缩内存
    vector<int>(v1).swap(v1);//利用匿名对象和拷贝构造
}
```



#### 3.2.8 vector预留空间

```c++
int main()
{
    vector<int>v;
    //reserve()预留空间,预留位置不初始化，无法访问
    v.reserve(1000);
}
```



### 3.3 deque容器

#### 3.3.1 deque基本概念

​	双端数组，头尾都可以插入删除操作

​	deque和vector的区别：

​		vector头部插入效率低

​		deque头部插入删除的速度比vector快

​		vector访问速度比deque快

​	deque内部工作原理：

​		deque内部有一个中控器，维护每段缓冲区中的内容，缓冲区存放真实内容

![](C:\Users\qingl\Downloads\微信截图_20220520134843.png)

​	deque的迭代器是支持随机访问的



#### 3.3.2 deque构造函数

```c++
void Print(const deque<int>& d)//const防止函数对传入参数修改，需要把迭代器改成const_iterator
{
    for(deque<int>::const_iterator it=d.Begin();it!=d.End();it++)
    {
        cout<<*it<<" ";
    }
    cout<<endl;
}

int main()
{
    //无参构造
    deque<int>d1;
    for(i=0;i<10;i++)
    {
        d1.push_back(i);
    }
    Print(d1);
    
    //区间构造
    deque<int>d2(d1.Begin(),d1.End());
    
    //几个几构造
    deque<int>d3(10,100);//10个100
    
    //拷贝构造
    deque<int>d4(d3);
}
```



#### 3.3.3 deque赋值操作

```c++
int main()
{
    deque<int>d1;
    //等号赋值
    deque<int>d2;
    d2=d1;
    //assign区间赋值
    d2.assign(d1.Begin(),d1.End());
    //assign几个几赋值
    d2.assign(10,100);
}
```



#### 3.3.4 deque大小操作

```c++
int main()
{
    deque<int>d1;
    //判断容器是否为空,返回bool
    d1.empty();
    //返回容器中元素的个数
    d1.size();
    //重新指定容器的长度，长度边长用0填充，变短删除末尾
    d1.resize(5);
    d1.resize(5,1);//用1填充
}
```



#### 3.3.5 deque插入删除

```c++
int main()
{
    deque<int>d1;
    //两端操作
    //头插
    d1.push_front(10);
    //尾插
    d1.push_back(20);
    //头删
    d1.pop_front();
    //尾删
    d1.pop_back();
    
    //指定位置操作
    //指定位置插入一个
    d1.insert(d1.Begin,100);
    //指定位置插入几个几
    d1.insert(d1.Begin(),2,100);
    //指定位置插入另一个区间的数
    deque<int>d2;
    d1.insert(d1.Begin(),d2.Begin(),d2.End());//第一个参数是位置，后面两个参数是区间，左闭右开
    //清空容器内容
    d1.clear();
    //删除指定区间，返回下一个数据位置
    it=d1.erase(d1.Begin(),d1.End());
    //删除指定位置，返回下一个数据位置
    deque<int>::iterator it=d.Begin();
    it++;//位移位置
    it=d1.erase(it);
}
```



#### 3.3.6 deque数据存取

```c++
int main()
{
    deque<int>d1;
    //[]访问指定位置元素
    d1[i];
    //at访问指定位置元素
    d1.at(i);
    //访问第一个元素
    d1.front();
    //访问最后一个元素
    d1.back();
}
```



#### 3.3.7 deque排序操作

```c++
#include<algorithm>//排序属于算法，要包含算法头文件
int main()
{
    deque<int>d1;
    sort(d1.Begin(),d1.End());//默认升序
}
```

​	注意：支持随机访问的迭代器的容器，都可以用sor算法直接对其进行排序   如vector也可以用sort排序



#### 补充：随机数

```c++
#include<ctime>//使用time需要包含该头文件
int main()
{
	srand((unsigned int)time(NULL));//随机数种子
    
    int a=rand()%41+60;//rand()返回0到RAND_MAX之间的随机数
}
```



### 3.5 stack容器

#### 3.5.1 stack基本概念

​	stack先进后出，只有一个出口

​	栈中只有栈顶元素可以被外界使用，因此==栈不允许有遍历行为==



#### 3.5.2 stack常用接口

```c++
#include<stack>
int main()
{
    stack<int>s;
    //入栈
    s.push(10);
    //出栈
    s.pop();
    //查看栈顶元素
    s.top();
    //判断栈是否为空
    s.empty();
    //判断栈大小
    s.size();
}
```



### 3.6 queue容器

#### 3.6.1 queue基本概念

​	队列 先进先出

​	只有队头和队尾才可以被外界使用，因此==队列不允许有遍历行为==



#### 3.6.2 queue常用接口

```c++
#include<queue>
int main()
{
    queue<int>q;
    //拷贝构造
    queue<int>q1;
    q1=q;
    //入队
    q.push(10);
    //出队
    q.pop();
    //第一个元素
    q.front();
    //最后一个元素
    q.back();
    //是否为空
    q.empty();
    //返回大小
    q.size();
}
```



### 3.7 list容器

#### 3.7.1 list基本概念

​	链表：将数据进行链式储存

​	STL中的链表是一个双向循环链表

![微信截图_20220520155012](C:\Users\qingl\Downloads\微信截图_20220520155012.png)

​		由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**

​		list优点：

​			采用**动态存储分配**，不会造成内存浪费和溢出

​			链表执行插入和删除方便

​		list缺点：

​			链表灵活，但空间（指针域）和时间（遍历）消耗较大

​		==list重要性质：插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的==



#### 3.7.2 list构造函数

```c++
#include<list>
int main()
{
    //无参构造
    list<int>l1;
    //赋值
    l1.push_back(10);
    //区间构造
    list<int>l2(l1.Begin(),l1.End());
    //拷贝构造
    list<int>l3(l1);
    //几个几构造
    list<int>l4(10,100);
}
```



#### 3.7.3 list赋值和交换

```c++
int main()
{
    list<int>l1;
    //等号赋值
    list<int>l2;
    l2=l1;
    //区间赋值
    list<int>l3;
    l3.assign(l1.Begin(),l1.End());
    //几个几赋值
    list<int>l4;
    l4.assign(10,100);
    
    //交换
    l1.swap(l2);
}
```



#### 3.7.4 list大小操作

```c++
int main()
{
    list<int>l1;
    //容器大小
    l1.size();
    //容器是否为空
    l1.empty();
    //容器重新指定大小，规则和vector等一样
    l1.resize(5);
    l1.resize(5,10);//5个10
}
```



#### 3.7.5 list插入删除

```c++
int main()
{
    list<int>l1;
    //头插
    l1.push_front(10);
    //尾插
    l1.push_back(20);
    //头删
    l1.pop_front();
    //尾删
    l1.pop_back();
    //插入
    l1.insert(l1.Begin(),100);
    //删除
    l1.erase(l1.Begin());
    //移除
    l1.remove(100);//删除所有是100的元素
    //清空
    l1.clear();
}
```



#### 3.7.6 list数据存取

```c++
int main()
{
    list<int>l1;
    //list不是连续线性空间存储数据，迭代器也不支持随机访问，因此没有[]和at()访问法
    //只能访问头和尾
    l1.front();
    l1.back();
    //验证迭代器不支持随机访问
    list<int>::iterator it=l1.Begin();
    it++;//合法
    it=it+1;//非法
}
```



#### 3.7.7 list反转和排序

```c++
void myCompare(int v1,int v2)
{
    return v1>v2;//降序就让第一个数大于第二个数
}
int main()
{
    list<int>l1;
    //反转
    l1.reverse();
    //排序
    sort(l1.Begin(),l1.End());//报错，对于不支持随机访问迭代器的容器，不可以用标准算法
    //不支持随机访问迭代器的容器，内部会提供对应算法
    l1.sort();//升序
    l1.sort(myCompare);//降序
}
```



#### 3.7.8 list自定义类排序

```c++
class Person
{
public:
    Person(int age,int height)
    {
        this->m_age=age;
        this->m_height=height;
    }
    int m_age;
    int m_height;
};

void myCompare(Person &p1,Person &p2)
{
    if(p1.m_age==p2.m_age)
    {
        return p1.m_height>p2.m_height;//年龄相同 按身高的降序
    }
    else
    {
        return p1.m_age<p2.m_age//年龄升序
    }
}

int main()
{
    list<Person>l1;
    l1.sort(myCompare);//通过自定义函数规定排序规则
}
```



### 3.8 set/multiset容器

#### 3.8.1 set基本概念

​	特点：所有元素都会在插入时被自动排序

​	本质：set/multiset属于关联式容器，底层结构是二叉树

​	set和multiset区别：

​		set不允许容器有重复元素

​		multiset允许容器中有重复元素



#### 3.8.2 set构造和赋值

```c++
#include<set>
int main()
{
    //无参构造
    set<int> st1;
    
    //拷贝构造
    set<int> st2(st1);
    
    //赋值
    st2=st1;
    
    //插入数据，只有insert方法
    st1.insert(10);
    //set容器会自动排序，不允许插入重复值
}
```



#### 3.8.3 set大小和交换

```c++
int main()
{
    set<int> s1;
    //大小
    s1.size();
    //是否为空
    s1.empty();
    //交换
    set<int> s2;
    s1.swap(s2);
}
```



#### 3.8.4 set插入删除

```c++
int main()
{
    set<int>s1;
    //插入
    s1.insert(10);
    //删除指定位置
    s1.erase(s1.Begin());
    //删除指定元素
    s1.erase(10);
    //删除区间
    s1.(s1.Begin(),s1.End());
}
```



#### 3.8.5 set查找统计

```c++
int main()
{
    set<int> s1;
    
    //查找，返回迭代器
    set<int>::iterstor pos=s1.find(30);
    
    //统计指定元素个数
    int num=s1.count(30);
}
```



#### 3.8.6 set和multiset区别

​	set不能元素重复  multiset可以元素重复

```c++
int main()
{
    set<int> s1;
    //insert会返回一个对组，第一个是插入位置的迭代器，第二个是是否插入成功
    pair<set<int>::iterator bool> ret=s1.insert(10);
}
```



#### 3.8.7 pair对组创建

​	特点：成对出现的数据，利用对组可以返回两个数据

```c++
//pair不用包含头文件
//创建对组
int main()
{
    //创建方法一
    pair<string,int>p1("aaa",111);
    //创建方法二
    pair<string,int>p2=make_pair("aaaa",111);
    //获取内容
    p1.first;
    p1.second;
}
```



#### 3.8.8 set容器排序

​	set容器默认从小到大排序，利用仿函数修改排序规则

```c++
class myCompare//仿函数本质上是一个类
{
    bool operator()(int v1,int v2)//重载()操作符
    {
        return v1>v2;
    }
};

int main()
{
    set<int,myCompare>s1;
}
```

​	==注意：自定义类型（如Person）都需要指定排序规则==



### 3.9 map/multimap容器

#### 3.9.1 map基本概念

​	特点：

​		map所有元素都是pair

​		pair中第一个元素为键值key，起到索引作用，第二个元素为实际值value

​		所有元素都会根据元素键值自动排序

​	本质：

​		map/multimap属于关联式容器，底层是二叉树

​	优点：

​		可以根据key值快速找到value值

​	map和multimap区别：

​		map不允许容器有重复key值

​		multimap允许容器有重复key值



#### 3.9.2 map构造和赋值

```c++
#include<map>
int main()
{
    //无参构造
    map<int,int>m1;
    //插入
    m1.insert(pair<int,int>(1,10));
    //拷贝构造
    map<int,int>m2(m1);
    //赋值
    map<int,int>m3;
    m3=m2;
    //访问
    int res=m1[20];
 }
```



#### 3.9.3 map大小和交换

```c++
int main()
{
    map<int,int> m1;
    //大小
    m1.size();
    //是否为空
    m1.empty();
    //交换
    map<int,int> m2;
    m1.swap(m2);
}
```



#### 3.9.4 map插入和删除

```c++
int main()
{
    map<int,int>m1;
    //第一种
    map.insert(pair<int,int>(1,10));//插入对组
    //第二种
    map.insert(make_pair(1,10));
    //第三种
    map.insert(map<int,int>::valuetype(1,10));
    //第四种
    m1[4]=10;//不建议，因为如果不存在key为4，则会多出一对pair(4,0)。但可以用[]访问map元素
    //删除
    //删除指定位置
    m1.erase(m1.Begin());
    //按照指定键值删除
    m1.erase(3);
    //删除区间
    m1.erase(m1.Begin(),m1.End());
    //清空
    m1.clear();
}
```



#### 3.9.5 map查找和统计

```c++
int main()
{
    map<int,int>m1;
    
    //按照键值查找，返回迭代器
    map<int,int>::iterstor pos=m1.find(3);
    
    //按照键值统计指定pair个数
    int num=m1.count(3);
}
```



#### 3.9.6 map容器排序

​	利用仿函数，改变map排序规则

```c++
class myCompare//仿函数本质上是一个类
{
    bool operator()(int v1,int v2)//重载()操作符,括号内是键值的类型
    {
        return v1>v2;
    }
};

int main()
{
     map<int,int,myCompare>m1;
}
```



## 4 STL 函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

​	概念：

​		==重载函数调用操作符的类，其对象被称为**函数对象**==

​		==**函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**==

​	本质：

​		函数对象（仿函数）是一个类，不是一个函数



#### 4.1.2 函数对象使用

​	特点：

​		1）函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值

​		2）函数对象超出普通函数的概念，函数对象可以有自己的状态

​		3）函数对象可以作为参数传递

```c++
class myAdd
{
	int num;//函数对象超出不同函数的概念，函数对象可以有自己的状态
    myAdd()
    {
        this->num=0;
    }
    //函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
    int operator()(int a,int b)
    {
        return a+b;
        num++;
    }
};

void Print(myAdd myadd)//函数对象可以作为参数传递
{
    //省略内容
}
    
int main()
{
    myAdd myadd;
    myadd(10,10);
    cout<<myadd.num<<endl;
    Print(myadd);
}
```



### 4.2 谓词

#### 4.2.1 谓词概念

​	概念：

​		==返回bool类型的仿函数叫谓词==

​		operator()()只接受一个参数，叫一元谓词

​		operator()()接受两个参数，叫二元谓词

```c++
//一元谓词
#include<algorithm>//find_if
class myFind
{
    bool operator()(int val)
    {
        return val>5;//查找容器有没有大于5的
    }
};
int main()
{
    vector<int>v1;
    //myFind()匿名函数对象
    vector<int>::iterator it=find_if(v1.begin(),v1.end(),myFind());
    if(it==v1.end())
    {
        //没找到
    }
    else
    {
        //找到
    }
}
```



```c++
//二元谓词
class myCompare
{
    bool operator()(int v1,int v2)
    {
        return v1>v2;
    }
};

int main()
{
    vector<int>v1;
    //myCompare()匿名函数对象
    sort(v1.begin(),b1.end(),myCompare());
}
```



### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

​	概念：STL内建了一些函数对象

​	分类：

​		算术仿函数

​		关系仿函数

​		逻辑仿函数

​	用法：

​		用法和一般函数完全相同

​		使用内建函数对象，需要引入头文件`#include<functional>`



#### 4.3.2 算术仿函数

​	功能：实现四则运算

```c++
#include<functional>
int main()
{
    //取反仿函数
    negate<int>(10);
    //加法仿函数
    plus<int>(10,20);
    //减法仿函数
    minus<int>(20,10);
    //乘法仿函数
    multiplies<int>(10,10);
    //除法仿函数
    divides<int>(10,2);
    //取模%仿函数
    modulus<int>(10,3);
}
```



#### 4.3.3 关系仿函数

```c++
int main()
{
    vector<int>v1;
    sort(v.begin(),v.end(),greater<int>());//大于，即降序排列
    //其他关系仿函数
    //等于
    equal_to<int>();
    //不等于
    not_equal_to<int>();
    //大于等于
    greater_equal<int>();
    //小于
    less<int>();
    //小于等于
    less_equal<int>();
}
```



#### 4.3.4 逻辑仿函数（不常用）

```c++
int main()
{
    vector<bool>v1;
    
    vector<bool>v2;
    v2.resize(v1.size());//搬运前必须要先指定大小
    transform(v1.begin(),v1.end(),v2.begin(),logical_not<bool>());//将容器v1搬运到容器v2中，并执行取反操作
    //其他逻辑仿函数
    //逻辑与
    logical_and<bool>();
    //逻辑或
    logical_or<bool>();
}
```



## 5 STL常用算法

概述：

​	算法主要是由头文件`<algorithm>,<functional>,<numeric>`组成

​	

### 5.1 常用遍历算法

#### 5.5.1 for_each

```c++
#include<algorithm>
void myPrint1(int val)//普通函数
{
    cout<<val<<" ";
}

class myPrint2//仿函数
{
    void operator()(int val)
    {
        cout<<val<<" ";
    }
};
int main()
{
    vector<int>v1;
    
    for_each(v.begin(),v.end(),myPrint1);//普通函数，不用加()
    for_each(v.begin(),v.end(),myPrint2());//仿函数，匿名函数对象，要加()
}
```



#### 5.1.2 transform

​	搬运容器到另一个容器

```c++
class Transform
{
    int operator()(int val)
    {
        return val+10;//搬运的同时可以进行一些操作
    }
};
int main()
{
    vector<int>v1;
    
    vector<int>v2;
    v2.resize(v1.size());//搬运前必须要先指定大小
    transform(v1.begin(),v1.end(),v2.begin(),Transform());
}
```



### 5.2 常用查找算法

#### 5.2.1 find

```c++
class Person
{
public:
    Person(int age,int height)
    {
        this->m_age=age;
        this->m_height=height;
    }
    int m_age;
    int m_height;
    //重载== 让底层find知道如何对比自定义类
    bool operator==(const Person &p)//const防止修改
    {
        if(this->m_age==p.m_age&&this->.m_height==p.m_height)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
};

int main()
{
    vector<Person>v1;
    Person p(111,222);//要找的对象
    vector<Person>::iterator it=find(v1.begin(),v1.end(),p);
    if(it==v1.end())
    {
        //没有找到
    }
    else
    {
        //找到
    }
}
```



#### 5.2.2 find_if

​	按条件查找元素

```c++
class Person
{
public:
    Person(int age,int height)
    {
        this->m_age=age;
        this->m_height=height;
    }
    int m_age;
    int m_height;
};

class Greater20
{
    bool operator()(const Person &p)//const防止修改
    {
        if(p.m_age>20)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
};

int main()
{
    vector<Person>v1;
    Person p(111,222);//要找的对象
    vector<Person>::iterator it=find_if(v1.begin(),v1.end(),Greater20());
    if(it==v1.end())
    {
        //没有找到
    }
    else
    {
        //找到
    }
}
```



#### 5.2.3 adjacent_find

​	查找相邻重复元素

```c++
int main()
{
    vector<int>v1;
    vector<int>::iterator it=adjacent_find(v1,begin(),v1.end());
    if(it==v1.end())
    {
        //没有找到
    }
    else
    {
        //找到
    }
}
```

​	注意：如果相邻重复元素有组的，就要从第一组的位置到最后在查找一次



#### 5.2.4 binary search（二分查找）

​	查找指定元素是否存在，**返回bool**

​	**无序序列不能用**

```c++
int main()
{
    vector<int>v1;//必须有序
    bool ret=binary_search(v1,begin(),v1.end(),9);
}
```



#### 5.2.5 count

​	统计元素个数

```c++
class Person
{
public:
    Person(int age,int height)
    {
        this->m_age=age;
        this->m_height=height;
    }
    int m_age;
    int m_height;
    //重载== 让底层find知道如何对比自定义类
    bool operator==(const Person &p)//const防止修改
    {
        if(this->m_age==p.m_age&&this->.m_height==p.m_height)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
};

int main()
{
    vector<Person>v1;
    Person p(111,222);//要找的对象
    int num=count(v1.begin(),v1.end(),p);
}
```



#### 5.2.6 count_if

​	按照条件统计元素个数

```c++
class Person
{
public:
    Person(int age,int height)
    {
        this->m_age=age;
        this->m_height=height;
    }
    int m_age;
    int m_height;
};

class Greater20
{
    bool operator()(const Person &p)//const防止修改
    {
        if(p.m_age>20)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
};

int main()
{
    vector<Person>v1;
    Person p(111,222);//要找的对象
    int num=find(v1.begin(),v1.end(),Greater20());
}
```



### 5.3 常用排序算法

#### 5.3.1 sort

```c++
int main()
{
    vector<int>v1;
    sort(v1.begin(),v1.end());//默认升序
    sort(v.begin(),v.end(),greater<int>());//大于，即降序排列
}
```



#### 5.3.2 random_shuffle

​	重新打乱顺序

```c++
int main()
{
    vector<int>v1;
    random_shuffle(v1,begin(),v1.end());
}
```



#### 5.3.3 merge

​	两个容器的元素合并，并储存到另一个容器

​	==两个容器必须是有序的==

```c++
int main()
{
    vector<int>v1;
    vector<int>v2;
    vector<int>targetv;
    v2.resize(v1.size()+v2.size);//必须要先指定大小
    merge(v1.begin(),v1.end(),v1.begin(),v2.end(),targetv.begin());
}
```



#### 5.3.4 reverse

​	容器内元素反转

```c++
int main()
{
    vector<int>v1;
    reverse(v1,begin(),v1.end());
}
```



### 5.4 常用拷贝和替换算法

#### 5.4.1 copy

```c++
int main()
{
    vector<int>v1;
    vector<int>v2;
    v2.resize(v1.size());
    copy(v1.begin(),v1.end(),v2.begin());
}
```



#### 5.4.2 replace

​	将指定范围内指定的旧元素改为新元素

```c++
int main()
{
    vector<int>v1;
    replace(v1.begin(),v1.end(),20,2000);//把区间内所有20替换成2000
}
```



#### 5.4.3 replace_if

​	将指定范围内满足条件的旧元素改为新元素

```c++
class Greater30
{
    bool operator()(int val)
    {
        return val>=30
    }
};

int main()
{
    vector<int>v1;
    replace_if(v1.begin(),v1.end(),Greater30(),3000);//把大于等于30的替换为3000
}
```



#### 5.4.4 swap

​	互换两个容器的元素

```c++
int main()
{
    vector<int>v1;
    vector<int>v2;
    swap(v1,v2);
}
```



### 5.5 常用算术生成算法

​	属于小型算法，使用时包含头文件`#include<numeric>`

#### 5.5.1 accumulate

```c++
#include<numeric>
int main()
{
    vector<int>v1;
    int total=accumulate(v1.begin(),v1.end(),0);//最后一个参数是起始累加值，有返回值
}
```



#### 5.5.2 fill

​	向容器内填充指定元素

```c++
int main()
{
    vector<int>v1;
    fill(v1.begin(),v1.end(),100);//用100填充
}
```



### 5.6 常用集合算法

#### 5.6.1 set_intersection

​	求两个容器的交集

​	==两个原容器必须是有序序列==

```c++
class myPrint
{
public:
    void operator()(int val)
    {
        cout<<val<<" ";
    }
};
int main()
{
	vector<int>v1;
    vector<int>v2;
    vector<int>targetv;
    targetv.resize(min(v1.size(),v2.size()));//min取最小值，要包含头文件algorithm
    vector<int>::iterator itEnd=set_intersection(v1.begin(),v1.end(),v2.begin(),v2.end(),targetv.begin());//返回结                                                                                                           束位置
    //遍历
    for_each(targetv.begin(),itEnd,myPrint());//要用itEnd,否则迭代器位置不对，会有多的0
    cout<<endl;
}
```



#### 5.6.2 set_union

​	求并集

​	两个容器必须是有序的

```c++
class myPrint
{
public:
    void operator()(int val)
    {
        cout<<val<<" ";
    }
};
int main()
{
	vector<int>v1;
    vector<int>v2;
    vector<int>targetv;
    targetv.resize(v1.size()+v2.size());
    vector<int>::iterator itEnd=set_union(v1.begin(),v1.end(),v2.begin(),v2.end(),targetv.begin());//返回结束位置
    //遍历
    for_each(targetv.begin(),itEnd,myPrint());//要用itEnd,否则迭代器位置不对，会有多的0
    cout<<endl;
}
```



#### 5.6.3 set_difference

​	求两个集合差集

```c++
class myPrint
{
public:
    void operator()(int val)
    {
        cout<<val<<" ";
    }
};
int main()
{
	vector<int>v1;
    vector<int>v2;
    vector<int>targetv;
    targetv.resize(max(v1.size(),v2.size()));//max取最大值，要包含头文件algorithm
    //v1和v2的差集
    vector<int>::iterator itEnd=set_difference(v1.begin(),v1.end(),v2.begin(),v2.end(),targetv.begin());//返回结束		位置
    //遍历
    for_each(targetv.begin(),itEnd,myPrint());//要用itEnd,否则迭代器位置不对，会有多的0
    cout<<endl;
    //v2和v1的差集
    vector<int>::iterator itEnd=set_difference(v2.begin(),v2.end(),v1.begin(),v1.end(),targetv.begin());
}
```

