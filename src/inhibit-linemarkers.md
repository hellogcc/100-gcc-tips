# 生成没有行号标记的预处理文件

## 技巧

有时编译程序会遇到如下类似的错误，

	In file included from foo.c:15,
	from a.h:45,
	b.h:53: error: ... ...

如果错误是由于你所定义的一个很复杂的宏所引起的，你可能会需要先手动编译生成相应的预处理文件，查看下预处理文件中的宏扩展代码。比如，先运行

	gcc -E foo.c -o foo.i

来生成foo.i预处理文件。然后，还可以尝试手动修改、编译这个预处理文件。

但是，由于生成的预处理文件中含有行号标记（linemarker），所以，运行

	gcc -c foo.i -o foo.o

所得到的错误行号信息还是跟最初的一样，如果可以将预处理文件中的行号标记都去掉，似乎会有些帮助。

幸好，gcc提供了这个选项：

> -P
> Inhibit generation of linemarkers in the output from the
> preprocessor. This might be useful when running the preprocessor on
> something that is not C code, and will be sent to a program which
> might be confused by the linemarkers.

运行

	gcc -E -P foo.c -o foo.i

即可。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#Preprocessor-Options)

## 贡献者

xmj

