# 设置动态连接器

## 技巧

有人问我，如何通过选项来指定动态连接器，而不使用缺省系统自带的动态连接器。我后来查了下ld的手册，有这么一个选项：

	-Ifile
	--dynamic-linker=file     
	    Set the name of the dynamic linker. This is only meaningful when generating dynamically linked ELF executables. The default dynamic linker is normally correct; don't use this unless you know what you are doing.

看起来，可以通过如下方式来完成：

	$ gcc foo.c -Wl,-I/home/xmj/tmp/ld-2.15.so
	$ ldd a.out
	linux-vdso.so.1 =>  (0x00007fffce5fe000)
	/usr/local/lib/libtrash.so (0x00007f1980477000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f19800a3000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f197fe9e000)
	/home/xmj/tmp/ld-2.15.so => /lib64/ld-linux-x86-64.so.2 (0x00007f1980485000)

注意，tmp目录下的动态连接器因为也是动态连接的，所以它本身是依赖系统缺省的动态连接器。

详情参见[ld手册](https://sourceware.org/binutils/docs-2.24/ld/Options.html#Options)

## 贡献者

xmj

