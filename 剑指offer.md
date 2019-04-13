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
