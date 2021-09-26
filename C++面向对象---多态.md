### C++面向对象---多态

++++++++++++++++++

```C++
Date:2021.9.14
Author:lqy
```

+ ### 基本特征

  + 分类：
    1. 静态多态：函数重载和运算符重载
    2. 动态多态：派生类和虚函数实现运行时多态
  + 区别：
    1. 静态多态：早绑定->编译时确定函数地址
    2. 动态多态：晚绑定->运行时确定函数地址

+ #### 多态实现

  + 多态存在条件：1. 存在有派生类和父类的**继承**关系 2. 子类对象**重写**父类中的**虚函数**
  + 多态实现形式：父类的**引用**或**指针**指向子类对象、
  + 多态父类虚函数：成员函数前加**virtual关键字**，实现地址晚绑定，以实现多态调用中执行子类中重写的成员函数
  
  ```c++
  class Animal
  {
  public:
  	virtual void speak()  // virtual虚函数（实现地址晚绑定，实现运行时确定函数地址）
  	{
  		cout << "动物在说话" << endl;
  	}
  };
  class Cat :public Animal
  {
  public:
  	void speak()
  	{
  		cout << "猫在说话" << endl;
  	}
  };
  
  void doSpeak(Animal& a)
  {
  	a.speak();
  }
  
  void test01()
  {
  	Cat cat;
  	doSpeak(cat);
  }
  
  /*
  1.参数列表 Animal &a->Cat cat（父类引用<同样可以使用指针>指向子类对象）
  2.public Animal实现子类与父类继承关系
  3.不添加virtual关键字，调用父类函数，添加virtual实现子类函数调用
  */
  ```
  
  

+ #### 多态内部机制：

+ #### 多态中纯虚函数和抽象类

  + 纯虚函数：由于多态实现，基类中虚函数多态机制中并不会真正执行，而只会执行子类中重写的函数，故省略函数体，简化代码

  + 抽象类：类中包含有**纯虚函数**的类称为**抽象类**

  + ##### Tips：

    1. **抽象类**无法通过任何方式**实例化类的对象**
    2. 子类继承抽象类时，必须“*重写父类中的纯虚函数*“，否则子类同样会被认定为**抽象类**

  + 代码示例：

    ```c++
    class Cook
    {
    public:
    	void Water()
    	{
    		cout << "煮水" << endl;
    	}
    	void Cup()
    	{
    		cout << "倒入杯中" << endl;
    	}
        /*
        1.C++中纯虚函数创建->virtual 返回值类型 函数名(参数列表) = 0;
        2.纯虚函数内部不用包含任何的函数语句，不需要执行任何操作
        */
    	virtual void Brew() = 0;
    	virtual void Add() = 0;
    
    };
    class Coffe:public Cook
    {
    public:
        /*
        多态实现：子类重写父类中的纯虚函数
        */
    	void Brew()
    	{
    		cout << "冲泡咖啡" << endl;
    	}
    	void Add()
    	{
    		cout << "加入糖和牛奶" << endl;
    	}
    };
    
    void test01()
    {
        // 多态实现：父类指针指向子类对象
    	Cook* cook = new Coffe;
    	cook->Water();
    	cook->Brew();
    	cook->Cup();
    	cook->Add();
        
        //new Coffe 在堆区来开辟内存空间，程序员手动创建，手动释放
        delete cook;
    }
    ```

+ #### 多态中”引用“与”指针“实现差异：

  + **引用**接受**类的实例化对象**,如果采用new'的方式开辟在堆区，需要解引用以传入实例化对象，同时采用"."方式调用函数

  + **指针**接受**类实例化对象的地址**，直接传入new出对象的地址接口，同时采用"->"的方式实现函数调用

  + Tips：

    <font color = Red>1. 区分”指针“”引用“的调用方式区别，重点在于知晓：**引用接受具体对象**，**指针接受对象地址**，同时在堆区开辟内存，**new的操作返回的为地址，而非具体对象**</font>

    <font color = Red>2. 堆区开辟的任何内存由程序员管理生与死，使用之后记得释放内存空间</font>

  ##### 示例代码：

  ```c++
  class Base
  {
  public:
  	virtual void func() = 0;
  };
  
  class Son1:public Base
  {
  public:
  	void func()
  	{
  		cout << "liqiyan" << endl;
  	}
  };
  
  class Son2 :public Base
  {
  public:
  	void func()
  	{
  		cout << "linxuan" << endl;
  	}
  };
  
  void test01()
  {
      /*
      1.new Son1->在堆区开辟内存空间，存放"new出来Son1对象的地址"
      2.引用须接受类实例化的对象，因此必须"解引用"传入实例化对象
      3.实例化对象采用"."的方式直接实现成员函数调用
      */
  	Base &b = *(new Son1); 
  	b.func();
      delete b;
  
      /*
      1.new Son2->在堆区开辟内存空间，存放"new出来Son2对象的地址"
      2.指针须接受类实例化的对象的地址，因此将new的内容直接传入即可
      3.指针采用"->"的方式直接实现成员函数调用
      */
  	Base* b1 = new Son2;
  	b1->func();
      delete b1;
  }
  void test02(Base& b)   // 传入实例化对象
  {
  	b.func();
  }
  void test03(Base* b)  // 传入指针，即实例化对象的地址
  {
  	b->func();
  }
  ```

+ #### 多态优点：

  实际工程开发中秉持**开闭原则**：

  **开：扩展开放，闭：修改闭合**->多态良好的实现此种程序开发原则，因此在合适的时机可以适当的利用多态技术进行程序开发

  1. 多态可以使代码的组织结构更加清晰
  2. 多态使得代码的可读性更强
  3. 多态利于程序前期与后期的扩展以及程序的维护性较高

+ #### 多态中虚析构与纯虚析构：

  + 应用场景：

    多态应用中，**子类对象**中拥有开辟在**堆区上的数据**，由程序员管理生存和释放，但<font color = Blue>父类对象的释放并不会调用子类的**析构函数**，导致子类中开辟在堆上的数据无法被释放，造成内存泄露</font>

  + 解决方案：

    + 父类析构函数改为**虚析构**或**纯虚析构函数**函数：

      <font color = Blue>**虚析构与纯虚析构函数仅保留其中之一即可**</font>
    
      ```C++
      class Animal
      {
      public:
      	virtual void speak() = 0;
      	Animal() {}
          
          ///*
          //父类中虚析构函数
          //*/
      	//virtual ~Animal() {}
      
      	/*
      	父类中纯虚析构函数，需注意纯虚函数声明与实现共同实现
      	*/
      	virtual ~Animal() = 0; // 函数声明
      
      };
      
      Animal::~Animal() {}    // 函数类外实现
      
      class Cat:public Animal
      {
      public:
      	void speak()
      	{
      		cout<<*catName << "在说话" << endl;
      	}
      	Cat(string name)
      	{
              /*
              提供猫子类的有参构造函数->参数catName开辟在堆区上，new一个string类型的对象并将其指向catName指针
              */
      		catName = new string(name);   
      	}
      	
      	~Cat()
      	{
              /*
              猫类的析构函数->用于释放开辟在堆区上的catName数据
              delete操作仅释放相关数据，指针依旧存在，为避免指针误指，故将指针置为空指针
              */
      		if (catName != NULL)
      		{
      			delete catName;
      			catName = NULL;
      		}
      	}
      public:
      	string* catName;   // 创建猫姓名的指针
      };
      
      void test01()
      {
      	Animal* animal = new Cat("Tom");   // 父类指针指向子类对象
      	animal->speak();
      	/*
      	* Cat的析构函数没有调用，导致Cat类中创建在堆区上的数据没被释放，导致堆区内存泄露
      	*/
      	delete animal;
      
      ```
    
  
+ #### 多态综合案例：

  说明：借助于**多态技术**实现电脑组装---电脑包含三个零件（1.CPU 2.显卡 3.内存条）

  ```c++
  /*
  定义三种零件（CPU、内存、显卡）的基类，作为父类待继承对象
  */
  class CPU
  {
  public:
  	virtual void calculator() = 0;
  };
  
  class videoCard
  {
  public:
  	virtual void display() = 0;
  };
  
  class Memory
  {
  public:
  	virtual void storge() = 0;
  };
  
  /*
  定义总体电脑类，用于组装电脑以及使电脑完成工作
  */
  class Computer
  {
  public:
  	Computer(CPU* cpu, videoCard* vc, Memory* mem)
      /*提供带参构造函数，实现电脑组装*/
  	{
  		this->cpu = cpu;
  		this->mem = mem;
  		this->vc = vc;
  	}
  	~Computer()
  	{
          /*由于后期零件相关数据开辟在堆上，需要程序员手动管理释放*/
  		if (this->cpu != NULL)
  		{
  			delete this->cpu;
  			this->cpu = NULL;
  		}
  
  		if (this->vc != NULL)
  		{
  			delete this->vc;
  			this->vc = NULL;
  		}
  
  		if (this->mem != NULL)
  		{
  			delete this->mem;
  			this->mem = NULL;
  		}
  	}
  
  	void work()
  	{
          /*电脑各部分调用类内成员方法，实现电脑具体功能*/
  		this->cpu->calculator();
  		this->vc->display();
  		this->mem->storge();
  	}
  private:
      /*
      提供电脑组装所需的三个零件->使用基类指针方式进行创建
      */
  	CPU* cpu;
  	videoCard* vc;
  	Memory* mem;
  };
  
  /*
  定义三种零件（CPU、显卡、内存）基类的子类，借助于多态技术，实现具体不同厂商提供的零件
  在继承基类的子类中重写父类中的虚函数，实现具体零件完成具体的工作
  */
  class IntelCPU:public CPU
  {
  	void calculator()
  	{
  		cout << "intel的cpu开始计算" << endl;
  	}
  };
  
  class IntelvideoCard :public videoCard
  {
  	void display()
  	{
  		cout << "intel的显卡开始显示" << endl;
  	}
  };
  
  class IntelMemory :public Memory
  {
  	void storge()
  	{
  		cout << "intel的存储器开始存储" << endl;
  	}
  };
  
  class LenovoCPU :public CPU
  {
  	void calculator()
  	{
  		cout << "Lenovo的cpu开始计算" << endl;
  	}
  };
  
  class LenovovideoCard :public videoCard
  {
  	void display()
  	{
  		cout << "Lenovo的显卡开始显示" << endl;
  	}
  };
  
  class LenovoMemory :public Memory
  {
  	void storge()
  	{
  		cout << "Lenovo的cpu开始计算" << endl;
  	}
  };
  
  
  void test01()
  {
      /*
      1. 创建电脑零件，数据开辟在堆上
      2. 借助于Computer的析构函数完成内存的释放
      */
  	CPU* intelCPU = new IntelCPU;
  	videoCard* intelCard = new IntelvideoCard;
  	Memory* memory = new IntelMemory;
  
      /*开辟电脑数据在堆上，同时调用有参构造函数实现电脑组装*/
  	Computer* computer = new Computer(intelCPU, intelCard, memory);
  	/*电脑工作*/
      computer->work();
  	/*清空开辟在堆上的Computer数据*/
      delete computer;
  
  }
  ```

  
