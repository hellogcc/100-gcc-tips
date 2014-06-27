# 生成有详细信息的汇编文件

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

使用`-fverbose-asm`选项就可以生成带有详细信息的汇编文件：

	$ gcc -S -fverbose-asm foo.c
	$ cat foo.s
		.file	"foo.c"
	# GNU C (Ubuntu/Linaro 4.6.3-1ubuntu5) version 4.6.3 (x86_64-linux-gnu)
	#	compiled by GNU C version 4.6.3, GMP version 5.0.2, MPFR version 3.1.0-p3, MPC version 0.9
	# GGC heuristics: --param ggc-min-expand=100 --param ggc-min-heapsize=131072
	# options passed:  -imultilib . -imultiarch x86_64-linux-gnu foo.c
	# -mtune=generic -march=x86-64 -fverbose-asm -fstack-protector
	# options enabled:  -fasynchronous-unwind-tables -fauto-inc-dec
	# -fbranch-count-reg -fcommon -fdelete-null-pointer-checks -fdwarf2-cfi-asm
	# -fearly-inlining -feliminate-unused-debug-types -ffunction-cse -fgcse-lm
	# -fident -finline-functions-called-once -fira-share-save-slots
	# -fira-share-spill-slots -fivopts -fkeep-static-consts
	# -fleading-underscore -fmath-errno -fmerge-debug-strings
	# -fmove-loop-invariants -fpeephole -fprefetch-loop-arrays
	# -freg-struct-return -fsched-critical-path-heuristic
	# -fsched-dep-count-heuristic -fsched-group-heuristic -fsched-interblock
	# -fsched-last-insn-heuristic -fsched-rank-heuristic -fsched-spec
	# -fsched-spec-insn-heuristic -fsched-stalled-insns-dep -fshow-column
	# -fsigned-zeros -fsplit-ivs-in-unroller -fstack-protector
	# -fstrict-volatile-bitfields -ftrapping-math -ftree-cselim -ftree-forwprop
	# -ftree-loop-if-convert -ftree-loop-im -ftree-loop-ivcanon
	# -ftree-loop-optimize -ftree-parallelize-loops= -ftree-phiprop -ftree-pta
	# -ftree-reassoc -ftree-scev-cprop -ftree-slp-vectorize
	# -ftree-vect-loop-version -funit-at-a-time -funwind-tables
	# -fvect-cost-model -fverbose-asm -fzero-initialized-in-bss
	# -m128bit-long-double -m64 -m80387 -maccumulate-outgoing-args
	# -malign-stringops -mfancy-math-387 -mfp-ret-in-387 -mglibc -mieee-fp
	# -mmmx -mno-sse4 -mpush-args -mred-zone -msse -msse2 -mtls-direct-seg-refs
	
	# Compiler executable checksum: 75e879ed14f91af504f4150eadeaa0e6
	
		.section	.rodata
	.LC0:
		.string	"%d "
		.text
		.globl	main
		.type	main, @function
	main:
	.LFB0:
		.cfi_startproc
		pushq	%rbp	#
		.cfi_def_cfa_offset 16
		.cfi_offset 6, -16
		movq	%rsp, %rbp	#,
		.cfi_def_cfa_register 6
		subq	$16, %rsp	#,
		movl	$0, -4(%rbp)	#, i
		jmp	.L2	#
	.L3:
		movl	$.LC0, %eax	#, D.2049
		movl	-4(%rbp), %edx	# i, tmp62
		movl	%edx, %esi	# tmp62,
		movq	%rax, %rdi	# D.2049,
		movl	$0, %eax	#,
		call	printf	#
		addl	$1, -4(%rbp)	#, i
	.L2:
		cmpl	$9, -4(%rbp)	#, i
		jle	.L3	#,
		movl	$10, %edi	#,
		call	putchar	#
		movl	$0, %eax	#, D.2050
		leave
		.cfi_def_cfa 7, 8
		ret
		.cfi_endproc
	.LFE0:
		.size	main, .-main
		.ident	"GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3"
		.section	.note.GNU-stack,"",@progbits

可以看到，在汇编文件中给出了gcc所使用的具体选项，以及汇编指令操作数所对应的源程序（或中间代码）中的变量。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Code-Gen-Options.html#index-fverbose-asm-2614)

## 贡献者

xmj

