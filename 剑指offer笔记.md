# 1.赋值运算符函数
## 要点：
* 返回值类型为**自身引用**`*this`,这样才可以连续赋值
* 把传入参数的类型声明为**常量引用**，这样可以不调用赋值构造函数，可以不改变输入的参数
* 是否**释放实例自身已有内存**。如果我p们忘记在分配新内存之前释放自身已有空间，则程序将出现**内存泄漏**,本题中有数据成员。指向字符串的指针：char* m_pDat。需要把指针指向的内存释放掉，再给指针声明为空。
* 判断传入的参数和当前的实例(`*this`)是不是同一个实例。如果是同一个，则不进行赋值操作，直接返回。**当this和传入的参数是同一个实例时，一旦释放自身内存，传入的参数的内存也被同时释放，因此再也找不到需要赋值的内容了。**
* 申请内存时可能会内存不足导致new char抛出异常，但是此时已经释放了mpDat指向的内存，并且指针为空，抛出异常时，原来类的实例**不再保持一个有效的状态，违背了异常安全性**。 解决：
> * 方法1.先用new分配新的内容，再delete释放已有内容。
> * 方法2.创建一个临时实例，再交换临时实例和原来的实例
## 记得写测试代码
* 把一个CMyString的实例赋值给另外一个实例
* 把一个CMyString的实例赋值给自己
* 连续赋值

## 代码：
```
class CMyString
{
public:
CMyString(char*  m_pData=nullptr);
CMyString(const  CMyString& str);
~CMyString(void);

private:
char* m_pData;
};

//代码：
CMyString& CMyString::operator=(const CMyString& str)
{
if(this==str)
{
CMyString strTemp(str);//复制构造函数创建临时对象，临时对象失效时会自动调用析构函数
char* pTemp=strTemp.m_pData;//创建一个指针指向临时对象的数据成员m_pData
strTemp.m_pData=m_pData;//交换
m_pData=pTemp;//交换
}
return *this;
}
```
# 2.实现Singleton模式（单例模式）

- singleton是设计模式里面的一个问题，**Singleton是唯一一个能够用短短几十行代码完整实现的模式**
- 其目的是使得类的一个对象成为系统中的唯一实例。
- 这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一对象的方式，可以直接访问，不需要实例化该类的对象。

## 要点：

单例模式的要点有三个：

- 单例类有且仅有一个实例
- 单例类必须自行创建自己的唯一实例
- 单例类必须集合所有其他对象提供这一实例

实现角度：

- 提供一个private构造函数（防止外部调用而构造类的实例），这样外部就无法构造实例
- 提供一个该类的static private对象
- 提供一个static public函数，用于创建或获取其本身的静态私有对象（例如：GenInstance()）

关键点：

- 线程安全（双检锁-DCL，即：double-checked locking）
- 资源释放

## UML结构图：

![](Data-structure-and-algorithm-notes-Chinese-/pic/singleton.png)

```C++
// singleton.h
#ifndef SINGLETON_H
#define SINGLETON_H

// 非真正意义上的单例
class Singleton
{
public:
    static Singleton& GetInstance()
    {
        static Singleton instance;
        return instance;
    }

private:
    Singleton() {}
};

#endif // SINGLETON_H
```

## ad



