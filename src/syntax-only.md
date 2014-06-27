# 只做语法检查

## 例子

	$ cat foo.c
	union u {
	  char c;
	  int i;
	}
	$ gcc -fsyntax-only foo.c
	foo.c:4:1: error: expected identifier or ‘(’ at end of input

## 技巧

如上所示，使用`-fsyntax-only`选项可以只做语法检查，不进行实际的编译输出。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-fsyntax-only-274)

## 贡献者

xmj

