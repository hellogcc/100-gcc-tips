# 打印头文件搜索路径

## 例子

	$ gcc -v foo.c
	...
	ignoring nonexistent directory "/usr/local/include/x86_64-linux-gnu"
	ignoring nonexistent directory "/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../../x86_64-linux-gnu/include"
	#include "..." search starts here:
	#include <...> search starts here:
	 /usr/lib/gcc/x86_64-linux-gnu/4.6/include
	 /usr/local/include
	 /usr/lib/gcc/x86_64-linux-gnu/4.6/include-fixed
	 /usr/include/x86_64-linux-gnu
	 /usr/include
	End of search list.
	...

## 技巧

如上所示，使用`-v`选项可以打印出gcc搜索头文件的路径和顺序。当然，也可以使用`-###`选项

## 贡献者

xmj

