## C++中浅拷贝与深拷贝

+++++++++++++++++

```C++
Date:2021.9.24
Author:lqy
```

+ #### 深浅拷贝：

  + 浅拷贝->简单的复制拷贝操作
  + 深拷贝->在的**堆区**重新申请空间，进行拷贝操作

+ #### 调用时机：

  当C++中class编写时，成员属性出现<font color = Red>堆区数据（即成员属性开辟在堆上，需要程序员手动释放）</font>，考虑深浅拷贝问题

+ #### 问题出现：

  ```C++
  // 编译器自动提供的拷贝构造函数
  	Person(const Person &p)
  	{
  		this->age = p.age;
  		this->height = p.height;   // 此时程序传递的为height指针（地址） 
  	}
  	~Person() {
          /*成员属性在堆区创建，需要程序员手动管理和释放，在析构函数中提供delete操作释放指针属性*/
  		if (height != NULL)
  		{
  			delete height;
  			height = NULL;
  		}
  	}
  ```

  ​          拷贝构造函数创建对象,假设利用编译器提供的拷贝构造函数，仅进行简单的值拷贝，<font color = Green>会将p1对象中的地址视作数据拷贝给p2对象，会造成p1和p2中的height指针指向同一个内存地址</font>，两个对象使用完毕调用两次析构函数，<font color = Green>会造成同一块内存地址的重复释放</font>，程序崩溃

+ #### 问题解决：

  ​         解决浅拷贝问题，需程序员<font color = Green>手动创建拷贝构造函数</font>，不调用编译器给定的构造函数，<font color = Yellow>在堆区重新开辟地址（与之前对象不同）来存储与原对象属性同样大小的数据</font>

  ```C++
  	Person(const Person &p)
  	{
          /*解决浅拷贝问题，手动创建拷贝构造函数，在堆区重新开辟地址（与之前对象不同）来存储同样大小的数据*/
  		this->age = p.age;
  		this->height = new int(*p.height);   //  创建与原对象大小相同的数据，仅地址不同
      }
  ```

+ 总体代码：

  ```c++
  class Person
  {
  public:
  	Person() {}
  	Person(int age,int height)
  	{
          /*class中包含以指针方式创建的成员属性，在堆区new数据赋值给该指针变量*/
  		this-> age = age;
  		this->height = new int(height); 
  	}
  	/*
  	// 编译器自动提供的拷贝构造函数
  	Person(const Person &p)
  	{
  		this->age = p.age;
  		this->height = p.height; 
  	}
  	*/
  	Person(const Person &p)
  	{
          /*解决浅拷贝问题，手动创建拷贝构造函数，在堆区重新开辟地址（与之前对象不同）来存储同样大小的数据*/
  		this->age = p.age;
  		this->height = new int(*p.height);
  	}
  	~Person() {
          /*成员属性在堆区创建，需要程序员手动管理和释放，在析构函数中提供delete操作释放指针属性*/
  		if (height != NULL)
  		{
  			delete height;
  			height = NULL;
  		}
  	}
  public:
  	int age;
  	int* height;  // 指针方式创建成员属性
  };
  
  void test01()
  {
  	Person p1(18,160);
  	cout << "p1的年龄为的：" << p1.age << "身高为：" << *p1.height << endl;
  	/*拷贝构造函数创建对象,假设利用编译器提供的拷贝构造函数，仅进行简单的值拷贝，会造成p1和p2中的height指针指向同一个内存地址，两个对象使用完毕调用两次析构函数，会造成同一块内存地址的重复释放，程序崩溃*/
      Person p2(p1);
  	cout << "p2的年龄为的：" << p2.age << "身高为：" << *p2.height << endl;
  ```









#### 额外收获：

1. 在没有成员属性开辟在堆区时，系统提供的默认析构函数即可满足程序要求
2. 在有成员属性开辟在堆区时，系统提供默认的构造函数无法满足程序需要，因为堆区数据需要程序员手动管理释放，因此需要程序员自己创建构造函数

```C++
~Person() {
        /*成员属性在堆区创建，需要程序员手动管理和释放，在析构函数中提供delete操作释放指针属性*/
		if (height != NULL)
		{
			delete height;
			height = NULL;
		}
	}
```

