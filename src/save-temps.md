# 保存临时文件

## 例子

	$ gcc -save-temps a/foo.c
	$ ls foo.*
	foo.c  foo.i  foo.o  foo.s

	$ gcc -save-temps=obj a/foo.c -o a/foo
	$ ls a
	foo  foo.c  foo.i  foo.o  foo.s

## 技巧

如上所示，使用选项`-save-temps`可以保存gcc运行过程中生成的临时文件。这些中间文件的名字是基于源文件而来，并且保存在当前目录下。

如果你在不同目录下有重名的源文件，那么中间文件就会有冲突了。此时，你可以使用`-save-temps=obj`来指定中间文件名基于目标文件而定，并保存在目标文件所在目录下。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html#Debugging-Options)

## 贡献者

xmj

