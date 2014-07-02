# 把选项传给连接器

## 例子

	#include <stdio.h>
	
	int main (void)
	{
	  puts ("Hello world!");
	  return 0;
	}

## 技巧

使用`-Wl,option`可以将选项`option`传递给连接器。

注意，逗号和选项之间不能有空格。一种常见用法，就是让连接器生成内存映射文件，例如：

	$ gcc -Wl,-Map=output.map foo.c
	$ cat output.map
	Archive member included because of file (symbol)
	
	/usr/lib/x86_64-linux-gnu/libc_nonshared.a(elf-init.oS)
	                              /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o (__libc_csu_init)
	
	Discarded input sections
	
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	 .gnu_debuglink
	                0x0000000000000000        0xc /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	 .gnu_debuglink
	                0x0000000000000000        0xc /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .note.GNU-stack
	                0x0000000000000000        0x0 /tmp/ccBOhdmq.o
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/x86_64-linux-gnu/libc_nonshared.a(elf-init.oS)
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	 .note.GNU-stack
	                0x0000000000000000        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	 .gnu_debuglink
	                0x0000000000000000        0xc /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	Memory map
	
	 ** file header
	                0x0000000000400000       0x40
	 ** segment headers
	                0x0000000000400040      0x1f8
	
	.interp         0x0000000000400238       0x1c
	 ** fill        0x0000000000400238       0x1c
	
	.note.ABI-tag   0x0000000000400254       0x20
	 .note.ABI-tag  0x0000000000400254       0x20 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	
	.note.gnu.build-id
	                0x0000000000400274       0x24
	 ** note header
	                0x0000000000400274       0x10
	 ** zero fill   0x0000000000400284       0x14
	
	.dynsym         0x0000000000400298       0x78
	 ** dynsym      0x0000000000400298       0x78
	
	.dynstr         0x0000000000400310       0x51
	 ** string table
	                0x0000000000400310       0x51
	
	.gnu.hash       0x0000000000400368       0x1c
	 ** hash        0x0000000000400368       0x1c
	
	.gnu.version    0x0000000000400384        0xa
	 ** versions    0x0000000000400384        0xa
	
	.gnu.version_r  0x0000000000400390       0x20
	 ** version refs
	                0x0000000000400390       0x20
	
	.rela.dyn       0x00000000004003b0       0x18
	 ** dynamic relocs
	                0x00000000004003b0       0x18
	
	.rela.plt       0x00000000004003c8       0x30
	 ** dynamic relocs
	                0x00000000004003c8       0x30
	
	.init           0x00000000004003f8       0x18
	 .init          0x00000000004003f8        0x9 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	                0x00000000004003f8                _init
	 .init          0x0000000000400401        0x5 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .init          0x0000000000400406        0x5 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	 .init          0x000000000040040b        0x5 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	.plt            0x0000000000400410       0x30
	 ** PLT         0x0000000000400410       0x30
	
	.text           0x0000000000400440      0x1d8
	 .text          0x0000000000400440       0x2c /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	                0x0000000000400440                _start
	 .text          0x000000000040046c       0x17 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	 ** fill        0x0000000000400483        0xd
	 .text          0x0000000000400490       0x92 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .text          0x0000000000400522       0x15 /tmp/ccBOhdmq.o
	                0x0000000000400522                main
	 ** fill        0x0000000000400537        0x9
	 .text          0x0000000000400540       0x92 /usr/lib/x86_64-linux-gnu/libc_nonshared.a(elf-init.oS)
	                0x0000000000400540                __libc_csu_init
	                0x00000000004005d0                __libc_csu_fini
	 ** fill        0x00000000004005d2        0xe
	 .text          0x00000000004005e0       0x36 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	 ** fill        0x0000000000400616        0x2
	 .text          0x0000000000400618        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	.fini           0x0000000000400618        0xe
	 .fini          0x0000000000400618        0x4 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	                0x0000000000400618                _fini
	 .fini          0x000000000040061c        0x5 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .fini          0x0000000000400621        0x5 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	.rodata         0x0000000000400628       0x11
	 ** merge constants
	                0x0000000000400628        0x4
	 .rodata        0x000000000040062c        0xd /tmp/ccBOhdmq.o
	
	.eh_frame       0x0000000000400640       0xa4
	 ** eh_frame    0x0000000000400640       0xa0
	 .eh_frame      0x00000000004006e0        0x4 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	
	.eh_frame_hdr   0x00000000004006e4       0x2c
	 ** eh_frame_hdr
	                0x00000000004006e4       0x2c
	
	.ctors          0x0000000000401e28       0x10
	 .ctors         0x0000000000401e28        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .ctors         0x0000000000401e30        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	
	.dtors          0x0000000000401e38       0x10
	 .dtors         0x0000000000401e38        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .dtors         0x0000000000401e40        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	                0x0000000000401e40                __DTOR_END__
	
	.jcr            0x0000000000401e48        0x8
	 .jcr           0x0000000000401e48        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .jcr           0x0000000000401e48        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	
	.dynamic        0x0000000000401e50      0x190
	 ** dynamic     0x0000000000401e50      0x190
	
	.got            0x0000000000401fe0        0x8
	 ** GOT         0x0000000000401fe0        0x8
	
	.got.plt        0x0000000000401fe8       0x28
	 ** GOT PLT     0x0000000000401fe8       0x28
	 ** GOT IRELATIVE PLT
	                0x0000000000402010        0x0
	 ** GOT         0x0000000000402010        0x0
	
	.data           0x0000000000402010       0x10
	 .data          0x0000000000402010        0x4 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	                0x0000000000402010                data_start
	                0x0000000000402010                __data_start
	 .data          0x0000000000402014        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	 .data          0x0000000000402018        0x8 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	                0x0000000000402018                __dso_handle
	 .data          0x0000000000402020        0x0 /tmp/ccBOhdmq.o
	 .data          0x0000000000402020        0x0 /usr/lib/x86_64-linux-gnu/libc_nonshared.a(elf-init.oS)
	 .data          0x0000000000402020        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	 .data          0x0000000000402020        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	.bss            0x0000000000402020       0x10
	 .bss           0x0000000000402020        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crt1.o
	 .bss           0x0000000000402020        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crti.o
	 .bss           0x0000000000402020       0x10 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtbegin.o
	 .bss           0x0000000000402030        0x0 /tmp/ccBOhdmq.o
	 .bss           0x0000000000402030        0x0 /usr/lib/x86_64-linux-gnu/libc_nonshared.a(elf-init.oS)
	 .bss           0x0000000000402030        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/crtend.o
	 .bss           0x0000000000402030        0x0 /usr/lib/gcc/x86_64-linux-gnu/4.6/../../../x86_64-linux-gnu/crtn.o
	
	.comment        0x0000000000000000       0x2b
	 ** merge strings
	                0x0000000000000000       0x2b
	
	.note.gnu.gold-version
	                0x0000000000000000       0x1c
	 ** note header
	                0x0000000000000000       0x10
	 ** fill        0x0000000000000010        0x9
	 ** zero fill   0x0000000000000019        0x3
	
	.symtab         0x0000000000000000      0x390
	 ** symtab      0x0000000000000000      0x390
	
	.strtab         0x0000000000000000      0x1d5
	 ** string table
	                0x0000000000000000      0x1d5
	
	.shstrtab       0x0000000000000000      0x115
	 ** string table
	                0x0000000000000000      0x115
	
详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html#Link-Options)

## 贡献者

xmj

