# 在命令行中取消宏定义

## 技巧

类似于`-D`选项，你可以使用`-U`选项在命令行中取消一个宏的定义，比如：

	$ gcc -U DEBUG macro.c

中间可以没有空格：

	$ gcc -UDEBUG macro.c

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#Preprocessor-Options)

## 贡献者

xmj

