### C++面向对象---封装

++++++++++++++

```c++
Date:2021.9.7
Author:lqy
```

+ #### 面向对象三大特征：

  <font color = Blue>封装，继承，多态</font>

+ #### 面向对象设计：

  **封装**：

  1. class(类)的定义：<font color = Red>成员属性，成员方法</font>
  2. class(类)对象的实例化：创建class的对象

  **需求**：定义一个圆类，在类内定义半径的成员属性，定义获得圆周长的成员方法

  ```C++
  // main()函数之外定义const常量->const double pi = 3.14;
  
  class Circle  // 类的定义----class 类名
  {
  public:
      /*成员属性*/
  	int r;
  public:
      /*成员方法*/
  	double calculator()
  	{
  		return 2 * pi * r;
  	}
  };
  
  void test01()
  {
  	Circle c;  // 类的“实例化”
  	/*类名可以通过"."的方式实现成员属性和成员方法的调用*/
      c.r = 10;
  	cout << "圆的周长为：" << c.calculator() << endl;
  }
  ```

+ #### 访问权限：

  + （1）public（公共权限）（2）protected（保护权限）（3）private（私有权限）
  + 权限的高低程度：<font color = Red>private>protected>public</font>
  + 三种权限成员属性，方法类内均可以进行访问， *类外访问仅能访问public变量，protected和private类外无法进行访问*
  + protected和private区别在于**继承**操作时，父类中protected权限的变量，子类可以访问，但private权限的变量，子类无法访问

+ #### struct 和 class区别：

  + struct和class均可以将对象**封装化**
  + <font color = Green>struct默认属性为public，class默认属性为private</font>

+ #### 成员属性私有化

  C++面向对象实际开发时，**类成员属性**一般情况下会设置为**私有属性**，同时<font color = Green>提供get、set方法实现私有属性的访问</font>

  优点：

  1. 私有属性类外不可以进行直接访问，避免类外对成员属性进行误操作
  2. get、set方法中可以设置筛选操作，保证用户输入的有效性

  ```C++
  class Person
  {
  private:
      /*Person class设置私有成员属性-private*/
  	string name;
  	int age;
  public:
      /*提供get、set方法实现类外访问成员私有属性*/
  	string getName()
  	{
  		return name;
  	}
  	void setName(string T_name)
  	{
  		name = T_name;
  	}
  	int getAge()
  	{
  		return age;
  	}
  	void setAge(int T_age)
  	{
          /*set函数内部设置筛选机制，保证用户输入的有效性*/
  		if (T_age > 0 && T_age < 150)
  		{
  			age = T_age;
  		}
  		else
  		{
  			age = 0;
  			cout << "您输入的年龄有误" << endl;
  		}
  	}
  };
  
  void test01()
  {
  	Person p;
  	p.setName("zhangsan");
  	p.setAge(20);
  	cout << "姓名是：" << p.getName() <<","<< "年龄是：" << p.getAge() << endl;
  }
  ```

+ #### 类作为函数参数：

  C++中面向对象创建的类，同样可以作为函数参数进入函数进行相关操作

  ```c++
  /*本例中Person类借助于上例中的Person，本例中不在重新编写*/
  bool isSame(Person &p1,Person &p2)
      /*以引用方式传入两个Person对象，调用Person的成员函数进行函数操作*/
  {
      if((p1.getName() == p2.getName())&&(p1.getAge() == p2.getAge()))
      {
          cout << "p1和p2相同" << endl;
      }else{
          cout << "p1和p2不相同" << endl;
      }
  }
  ```

+ #### 类作为类的成员属性：

  需求：设计圆类，包含**圆心**和**半径**

  解决方案：设计点(Pointer)类，将其作为圆类的圆心属性传入Circle calss中

  ```c++
  class Pointer
      /*点类->后期作为圆心传入Circle类中*/
  {
  private:
      /*点的横纵(x,y坐标)设置为私有属性*/
  	int x;
  	int y;
  public:
      /*提供对外public权限的get、set方法实现横纵坐标的获取*/
  	void setX(int T_x)
  	{
  		x = T_x;
  	}
  	int getX()
  	{
  		return x;
  	}
  	void setY(int T_y)
  	{
  		y = T_y;
  	}
  	int getY()
  	{
  		return y;
  	}
  };
  
  class Circle
  {
      /*设计圆类*/
  private:
  	int r;
  	Pointer p;   // 将class Pointer作为成员属性传入
  public:
  	void setR(int T_r)
  	{
  		r = T_r;
  	}
  	int getR()
  	{
  		return r;
  	}
      /*提供类成员属性的get、set方法*/
  	void setPointer(Pointer& T_p)  // 以引用方式传入Pointer类型的实例化对象
  	{
  		p = T_p;
  	}
  	Pointer getPointer()          // 返回对象的类型为自定义数据类型-Pointer
  	{
  		return p;
  	}
  };
  ```

+ #### C++中类的分文件编写

  利用C++进行大型项目创建时，多个类编写在同一代码文件中代码的维护成本高，可读性差，在此种情况下，利用**类的分文件**编写，可以在其他文件中创建类，实现类的分文件编写

  步骤：

  1. 添加.h头文件，并编写类的成员属性以及成员方法的声明
  2. 添加.cpp文件，并编写类成员方法的具体实现,注意函数作用域的添加
  3. 在main函数文件中添加头文件，创建实例化对象

  ```c++
  //以上述Pointer为例，演示类的分文件编写
  
  /*添加.h头文件，并编写类的成员属性以及成员方法的声明*/
  //Pointer.h文件
  #pragma once  // 防止头文件重复包含
  #include<iostream>
  using namespace std;
  
  class Pointer
  {
  	/*.h文件中仅需包含成员属性和成员方法的声明，不需要具体的实现*/
  private:
  	int x;
  	int y;
  public:
  	/*成员方法去除函数体，后面添加分号*/
  	void setX(int T_x);
  	int getX();
  	void setY(int T_y);
  	int getY();
  };
  
  /*添加.cpp文件，并编写类成员方法的具体实现*/
  //Pointer.cpp文件
  #include "Pointer.h"   // 包含头文件(注意使用双引号，而非尖括号)
  /*
  * 1. .cpp文件中添加函数具体实现
  * 2. 函数实现增加作用域，保证其作为局部函数
  */
  
  void Pointer::setX(int T_x)
  {
  	x = T_x;
  }
  int Pointer::getX()
  {
  	return x;
  }
  void Pointer::setY(int T_y)
  {
  	y = T_y;
  }
  int Pointer::getY()
  {
  	return y;
  }
  
  /*在main函数文件中添加头文件，创建实例化对象*/
  //test.cpp文件
  #include<iostream>
  using namespace std;
  #include "Pointer.h"  // 包含头文件(注意使用双引号，而非尖括号)
  
  void test01()
  {
      Pointer p;
  }
  
  ```

+ #### C++中对象初始化和清理：

  1. C++中对象的初始化和清理借助于**构造函数**和**析构函数**完成
  2. 程序员不提供构造和析构函数，编译器会**自动提供**，但编译器提供的构造函数和析构函数是<font color = Red>空实现</font>
  3. 程序员手动提供构造函数和析构函数，编译器就不会提供默认的构造和析构函数

  构造函数和析构函数语法：

  构造函数-><font color = Red>类名() {}</font>（构造函数在创建类的实例化对象时会被调用）

  析构函数-><font color = Red>~类名() {}</font>(析构函数调用时机是在实例化对象销毁时调用，开辟在栈上的数据程序退出即被销毁，开辟在堆区的数据需要程序员手动管理生死，因此在程序员delete对象时销毁对象)

  **Tips**:

  1. 构造函数和析构函数，没有返回值，也不写void
  2. 函数名称必须与类名相同
  3. 程序会自动调用构造和析构函数，无需手动调用，且只会调用一次
  4. 构造函数可以带有参数，即发生函数重载，而析构函数不能发生函数重载，即析构函数不能存在参数

  ```C++
  #include<iostream>
  using namespace std;
  
  class Person
  {
  public:
  	/*无参构造和有参构造函数*/
  	Person() {}
  	Person(string name, int age)
  	{
          /*this指代所创建对象的指针*/
  		this->name = name;
  		this->age = age;
  	}
  	/*析构函数*/
  	~Person() {}
  private:
  	string name;
  	int age;
  };
  
  void test01()
  {
      /*执行test01()函数，Person的构造和析构函数均会被调用*/
  	/*Person对象开辟在栈区，test01()函数调用完成后即释放对象*/
  	Person p;
  }
  
  int main()
  {
      /*执行main()函数，此时仅会调用Person的构造函数，而不会调用析构函数*/
      /*由于main()函数中包含system("pause")代码，程序会停在此位置，如果按下任意键，程序退出，Person对象释放，析构函数调用*/
      Person p1;
      
      system("pause");
      return 0;
  }
  ```

+ #### C++中的构造函数

  + 按照**参数**分类：有参构造和无参构造

    ```c++
    /*有参和无参构造函数*/	
    Person() {}
    Person(string name,int age)
    {
        this->name = name;
        this->age = age;
    }
    ```

    

  + 按照**类型**分类：普通构造和拷贝构造

    ```C++
    //上述有参和无参构造函数均属于普通构造函数
    Person(const Person& p)
    {
    /*拷贝构造函数->1. 参数为同class的Person对象 2. Person利用const进行修饰，防止误修改 3. Person利用引用的方式传递*/
    this->name = p.name;
    this->age = p.age;
    }
    ```

  + 构造函数调用方式：

    1. <font color = Red>括号法</font>

       ```C++
       Person p("liqiyan",11);
       ```

    2. 显示法

    3. 隐式转换法

  + C++的匿名对象：

    利用括号法初始化**匿名对象**

    ```C++
    Person("liqiyan",10);
    ```

+ #### C++中构造函数调用时机

  + 已经创建完毕的对象初始化另外一新对象

  + 值传递方式给函数参数传值

    ```C++
    void doWork(Person p)
    /*值传递拷贝临时副本，不会对原值造成影响*/
    {}
    
    void test02()
    {
    	Person p;  
    	doWork(p);
        /*此时作为函数参数进行的传递的并非p本身，而是根据p对象拷贝出来的新的样本副本*/
    }
    ```

  + 值的方式返回局部对象

    ```C++
    Person doWork2()
    {
    	Person p1;
    	return p1;
        /*此处返回的并非p1对象本身，而是根据骄拷贝构造函数复制出来的副本*/
    }
    ```

+ #### 构造函数调用规则：

  + C++类的默认函数：
    1. **默认构造函数**（无参，函数体为空）
    2. **默认析构函数**（无参，函数体为空）
    3. **拷贝构造函数**（值拷贝）
  + C++默认函数调用规则：
    1. 自行添加有参构造函数，C++不会提供默认构造，但依旧提供拷贝构造函数
    2. 自行添加拷贝构造函数，C++不会提供默认构造和有参构造

