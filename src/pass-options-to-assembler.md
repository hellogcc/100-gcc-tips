# 把选项传给汇编器

## 例子

	#include <stdio.h>
	
	int main(void)
	{
	  int i;
	
	  for (i = 0; i < 10; i++)
	    printf("%d ", i);
	  putchar ('\n');
	
	  return 0;
	}

## 技巧

使用`-Wa,option`可以将选项`option`传递给汇编器。

注意，逗号和选项之间不能有空格。例如：

	$ gcc -c -Wa,-L foo.c
	$ objdump -d foo.o
	
	foo.o:     file format elf64-x86-64
	
	
	Disassembly of section .text:
	
	0000000000000000 <main>:
	   0:   55                      push   %rbp
	   1:   48 89 e5                mov    %rsp,%rbp
	   4:   48 83 ec 10             sub    $0x10,%rsp
	   8:   c7 45 fc 00 00 00 00    movl   $0x0,-0x4(%rbp)
	   f:   eb 1b                   jmp    2c <.L2>
	
	0000000000000011 <.L3>:
	  11:   b8 00 00 00 00          mov    $0x0,%eax
	  16:   8b 55 fc                mov    -0x4(%rbp),%edx
	  19:   89 d6                   mov    %edx,%esi
	  1b:   48 89 c7                mov    %rax,%rdi
	  1e:   b8 00 00 00 00          mov    $0x0,%eax
	  23:   e8 00 00 00 00          callq  28 <.L3+0x17>
	  28:   83 45 fc 01             addl   $0x1,-0x4(%rbp)
	
	000000000000002c <.L2>:
	  2c:   83 7d fc 09             cmpl   $0x9,-0x4(%rbp)
	  30:   7e df                   jle    11 <.L3>
	  32:   bf 0a 00 00 00          mov    $0xa,%edi
	  37:   e8 00 00 00 00          callq  3c <.L2+0x10>
	  3c:   b8 00 00 00 00          mov    $0x0,%eax
	  41:   c9                      leaveq 
	  42:   c3                      retq 

这里的`-L`是汇编器as的选项，用于在目标文件中保留局部符号（local symbol）。可以看到，反汇编代码中给出了每个局部符号。

如果此时你使用`oprofile`来统计性能事件，那么获得的结果将不是以函数为单位了，而是以这些符号所划分的代码块为单位。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Assembler-Options.html#Assembler-Options)和[as手册](https://sourceware.org/binutils/docs-2.24/as/L.html#L)

## 贡献者

xmj

