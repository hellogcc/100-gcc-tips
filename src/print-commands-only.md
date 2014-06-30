# 打印gcc执行的子命令

## 例子

	$ gcc -### foo.c
	Using built-in specs.
	COLLECT_GCC=gcc
	COLLECT_LTO_WRAPPER=/usr/lib/gcc/x86_64-linux-gnu/4.6/lto-wrapper
	Target: x86_64-linux-gnu
	Configured with: ../src/configure -v --with-pkgversion='Ubuntu/Linaro 4.6.3-1ubuntu5' --with-bugurl=file:///usr/share/doc/gcc-4.6/README.Bugs --enable-languages=c,c++,fortran,objc,obj-c++ --prefix=/usr --program-suffix=-4.6 --enable-shared --enable-linker-build-id --with-system-zlib --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --with-gxx-include-dir=/usr/include/c++/4.6 --libdir=/usr/lib --enable-nls --with-sysroot=/ --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --enable-gnu-unique-object --enable-plugin --enable-objc-gc --disable-werror --with-arch-32=i686 --with-tune=generic --enable-checking=release --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu
	Thread model: posix
	gcc version 4.6.3 (Ubuntu/Linaro 4.6.3-1ubuntu5) 
	COLLECT_GCC_OPTIONS='-mtune=generic' '-march=x86-64'
	 /usr/lib/gcc/x86_64-linux-gnu/4.6/cc1 -quiet -imultilib . -imultiarch x86_64-linux-gnu foo.c -quiet -dumpbase foo.c "-mtune=generic" "-march=x86-64" -auxbase foo -fstack-protector -o /tmp/ccezMraJ.s
	COLLECT_GCC_OPTIONS='-mtune=generic' '-march=x86-64'
	 as --64 -o /tmp/cc9Ce7IE.o /tmp/ccezMraJ.s
	COMPILER_PATH=/usr/lib/gcc/x86_64-linux-gnu/4.6/:/usr/lib/gcc/x86_64-linux-gnu/4.6/:/usr/lib/gcc/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/4.6/:/usr/lib/gcc/x86_64-linux-gnu/
	LIBRARY_PATH=/home/xmj/install/cap-llvm-3.4/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/4.6/:/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../../lib/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/home/xmj/install/cap-llvm-3.4/lib/:/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../:/lib/:/usr/lib/
	COLLECT_GCC_OPTIONS='-mtune=generic' '-march=x86-64'
	 /usr/lib/gcc/x86_64-linux-gnu/4.6/collect2 "--sysroot=/" --build-id --no-add-needed --as-needed --eh-frame-hdr -m elf_x86_64 "--hash-style=gnu" -dynamic-linker /lib64/ld-linux-x86-64.so.2 -z relro /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o -L/home/xmj/install/cap-llvm-3.4/lib/../lib -L/usr/lib/gcc/x86_64-linux-gnu/4.6 -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../../lib -L/lib/x86_64-linux-gnu -L/lib/../lib -L/usr/lib/x86_64-linux-gnu -L/usr/lib/../lib -L/home/xmj/install/cap-llvm-3.4/lib -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../.. /tmp/cc9Ce7IE.o -lgcc --as-needed -lgcc_s --no-as-needed -lc -lgcc --as-needed -lgcc_s --no-as-needed /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o

## 技巧

如上所示，使用`-###`选项可以打印出gcc所执行的各个子命令，分别为，

cc1：

	 /usr/lib/gcc/x86_64-linux-gnu/4.6/cc1 -quiet -imultilib . -imultiarch x86_64-linux-gnu foo.c -quiet -dumpbase foo.c "-mtune=generic" "-march=x86-64" -auxbase foo -fstack-protector -o /tmp/ccezMraJ.s

as：

	 as --64 -o /tmp/cc9Ce7IE.o /tmp/ccezMraJ.s

collect2：

	 /usr/lib/gcc/x86_64-linux-gnu/4.6/collect2 "--sysroot=/" --build-id --no-add-needed --as-needed --eh-frame-hdr -m elf_x86_64 "--hash-style=gnu" -dynamic-linker /lib64/ld-linux-x86-64.so.2 -z relro /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o -L/home/xmj/install/cap-llvm-3.4/lib/../lib -L/usr/lib/gcc/x86_64-linux-gnu/4.6 -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../../../lib -L/lib/x86_64-linux-gnu -L/lib/../lib -L/usr/lib/x86_64-linux-gnu -L/usr/lib/../lib -L/home/xmj/install/cap-llvm-3.4/lib -L/usr/lib/gcc/x86_64-linux-gnu/4.6/../../.. /tmp/cc9Ce7IE.o -lgcc --as-needed -lgcc_s --no-as-needed -lc -lgcc --as-needed -lgcc_s --no-as-needed /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o

这个跟使用`-v`所显示的内容差不多，区别在于使用`-###`是只打印，不实际执行具体的命令。手册里提到，它的一种用法，就是在脚本里使用这个选项，来获得gcc所调用的各个子命令行。


详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Overall-Options.html#Overall-Options)

## 贡献者

xmj

