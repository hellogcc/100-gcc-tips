# 指定语言类型

## 技巧

gcc是通过文件名后缀来判断源代码语言类型的。

如果你从标准输入把源码传给gcc，那么就需要通过`-x`选项显式的指定语言类型：

	$ echo "int x;" | gcc -S -x c -
	$ cat ./-.s
		.file	""
		.comm	x,4,4
		.ident	"GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3"
		.section	.note.GNU-stack,"",@progbits

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html#Overall-Options)

## 贡献者

xmj

