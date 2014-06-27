# error: cast from ... to ... loses precision

## 例子

	#include <iostream>
	
	class Foo {
	 public:
	  void print() const {
	    std::cout << (int)(this) << "\n";
	  }
	};
	
	int main()
	{
	  class Foo foo;
	
	  foo.print();
	  return 0;
	}

## 技巧

在g++编译上面的例子，会报如下错误：

	$ g++ foo.cc 
	foo.cc: In member function ‘void Foo::print() const’:
	foo.cc:6:28: error: cast from ‘const Foo*’ to ‘int’ loses precision [-fpermissive]

这是一个强制类型转换的错误，你可以修改源代码为：

	std::cout << (int*)(this) << "\n";

即可。

如果，你不想（或不能）去修改源程序，只是应为升级了gcc而带来了这样的错误，那么也可以使用`-fpermissive`选项，将错误降低为警告：

	$ g++ foo.cc -fpermissive
	foo.cc: In member function ‘void Foo::print() const’:
	foo.cc:6:28: warning: cast from ‘const Foo*’ to ‘int’ loses precision [-fpermissive]

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/C_002b_002b-Dialect-Options.html#index-fpermissive-166)

## 贡献者

xmj

