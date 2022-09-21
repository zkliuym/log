# [typename和class的区别](https://github.com/zkliuym/log/issues/6)

在c++ Template中很多地方都用到了typename与class这两个关键字，而且好像可以替换，是不是这两个关键字完全一样呢?

相信学习C++的人对class这个关键字都非常明白，class用于定义类，在模板引入c++后，最初定义模板的方法为： template……

在这里class关键字表明T是一个类型，后来为了避免class在这两个地方的使用可能给人带来混淆，所以引入了typename这个关键字，它的作用同

class一样表明后面的符号为一个类型，这样在定义模板的时候就可以使用下面的方式了：

template……

在模板定义语法中关键字class与typename的作用完全一样。

typename难道仅仅在模板定义中起作用吗？其实不是这样，typename另外一个作用为：使用嵌套依赖类型(nested depended name)，如下所示：

`
class MyArray 
{ 
  public：
  typedef int LengthType;
  .....
}

template<class T>
void MyMethod( T myarr ) 
{ 
  typedef typename T::LengthType LengthType; 
  LengthType length = myarr.GetLength; 
}
`

这个时候typename的作用就是告诉c++编译器，typename后面的字符串为一个类型名称，而不是成员函数或者成员变量，这个时候如果前面没有typename，编译器没有任何办法知道T::LengthType是一个类型还是一个成员名称(静态数据成员或者静态函数)，所以编译不能够通过。

原文链接：https://blog.csdn.net/newbeixue/article/details/116591068
