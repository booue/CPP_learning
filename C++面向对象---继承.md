### C++面向对象---继承

++++++++

```c++
Date:2021.9.14
Author:lqy
```

面向对象之继承：

1. 适用情况：多class具有相同的属性或行为，采用继承方法，将多class共有的部分（属性，方法）提取出来作为父类，其他作为派生类继承父类
2. 优点：减少代码重复性，增强代码可读性



+ #### 继承实现方式：

  **继承语法**---class 父类：继承方式 子类

  ```c++
  // 父类
  class BasePage
  {};
  
  // 派生类
  class JavaPage:public BasePage
  {};
  ```

+ #### 继承方式区别：

  **继承方式**---"公共继承->public、“保护继承->protected”、“私有继承->private”

  ```c++
  class Base
  {
  public:           // 公众权限
  	int num1;
  protected:        // 保护权限
  	int num2;
  private:          // 私有继承
  	int num3;
  };
  
  /*
  继承方式总原则：权限程度上升
  */
  class Son1{}:public Base    // 所有父类成员权限保持不变
  class Son2{}:protected Base // 所有父类成员权限低于protected上升为protected，高于protected的保持不变
  class Son3{}:private Base   // 所有父类成员权限全部变为private
  ```

  **Tips**：

  1. **权限程度**---private>protected>public
  2. **类外访问父类属性**，仅能访问public权限的属性变量和方法，protected和private均无法进行访问
  3. **类内访问父类属性**，继承的变量，其中public和protected可以类内访问，父类private属性无法在子类中访问
  4. **类内访问自身属性**，可以进行自由访问，包括protected和private以及public属性的变量
  5. **类外访问自身属性**，自身变量，也仅public可以类外访问，protected、private变量无法类外访问

+ #### 继承对象模型

  **对象模型**---父类和子类成员属性全部属于子类对象模型中，<font color = Red> 无论父类中成员属性权限为public，protected，private</font>

  **Tips**:父类中private权限的成员属性，同样被子类继承下来，只是被编译器隐藏，子类访问不到而已

+ #### 继承中的构造和析构顺序

  子类创建时，继承操作同样会创建父类对象

  **构造函数**：先有父类构造函数，再有子类构造函数

  **析构函数**：先有子类析构函数，再有父类析构函数

+ #### 继承同名成员

  + **子类属性、方法**：直接点的方式即可
  + **父类属性、方法**：添加作用域访问

  ```c++
  class Father
  {
  public:
  	int num;
      void func()
      {
          cout << endl;
      }
  };
  
  class Son
  {
  public:
  	int num;
          void func()
      {
          cout << endl;
      }
  };
  
  /*
  子类对象访问父类中同名的成员属性或方法，添加作用域
  */
  Son s;
  // 子类属性->s.num
  // 父类属性->s.Father::num
  
  // 子类方法->s.func()
  // 父类方法->s.Father::func()
  ```

+ #### C++多继承

  多继承语法：

  ```c++
  // C++支持多继承机制，子类访问父类中元素添加作用域
  
  class Son:public Base1,public Base2
  {};
  ```

+ #### 菱形继承/钻石继承

  描述：两个子类继承同一个基类（Base类），另外存在一个类同时继承该两个子类

  问题：子类会**继承两份相同的数据**，导致资源浪费，同时变量会存在**二义性**

  解决方法：**虚继承->virtual 关键字**

  ```c++
  class Animal
  {
  public:
  	int age;
  };
  class Sheep :virtual public Animal
  {};
  class Tuo :virtual public Animal
  {};
  class Yangtuo :public Sheep, public Tuo
  {};
  
  void test01()
  {
  	Yangtuo yg;
  
  	/*
  	* 添加作用域完成区分
  	*/
  	yg.Sheep::age = 18;
  	yg.Tuo::age = 20;
  
  	cout << yg.Tuo::age << endl;
  	cout << yg.Sheep::age << endl;
  	cout << yg.age << endl;      
      /*
      此时因为内存中仅存在一份数据，不会出现二义性，故采用三种方法获得的数据一致，且以最终更新的数据为准
      */
      
      // 虚基类指针->指向子类自身的虚基类表（存储地址偏移量，指针指向唯一的数据）
  }
  ```

  

