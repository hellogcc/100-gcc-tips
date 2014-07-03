# all warnings being treated as errors

## 技巧

在ubuntu系统下编译一个程序包，有时会遇到这样的错误：

	$ make
	...
	cc1: all warnings being treated as errors

这是因为缺省的CFLAGS里含有`-Werror`选项，将警告信息升级为错误。当然，一方面这可以让你重视这些可能会带来隐患的警告信息；但，如果你不想修改源码，也可以把这个选项关掉，通过修改Makefile或者使用命令行：

	$ make CFLAGS="... -Wno-error"

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#Preprocessor-Options)

## 贡献者

xmj

