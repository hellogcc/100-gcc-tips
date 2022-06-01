# 打印连接库的具体路径

## 例子

	$ gcc -print-file-name=libc.a
	/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/libc.a

## 技巧

如上所示，使用`-print-file-name`选项就可以显示出gcc究竟会连接哪个libc库了。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Developer-Options.html#index-print-file-name)

## 贡献者

xmj

