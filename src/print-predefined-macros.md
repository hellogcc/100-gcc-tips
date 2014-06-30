# 打印gcc预定义的宏信息

## 例子

	[root@linux:~]$ gcc -dM -E - < /dev/null
	#define __DBL_MIN_EXP__ (-1021)
	#define __FLT_MIN__ 1.17549435e-38F
	#define __CHAR_BIT__ 8
	#define __WCHAR_MAX__ 2147483647
	#define __GCC_HAVE_SYNC_COMPARE_AND_SWAP_1 1
	#define __GCC_HAVE_SYNC_COMPARE_AND_SWAP_2 1
	#define __GCC_HAVE_SYNC_COMPARE_AND_SWAP_4 1
	#define __DBL_DENORM_MIN__ 4.9406564584124654e-324
	#define __GCC_HAVE_SYNC_COMPARE_AND_SWAP_8 1
	#define __FLT_EVAL_METHOD__ 0
	#define __unix__ 1
	#define __x86_64 1
	#define __DBL_MIN_10_EXP__ (-307)
	#define __FINITE_MATH_ONLY__ 0
	#define __GNUC_PATCHLEVEL__ 7


## 技巧

如上所示，使用“`gcc -dM -E - < /dev/null`”命令就可以显示出gcc预定义的宏信息。“`-dM`”生成预定义的宏信息，“`-E`”表示预处理操作完成后就停止，不再进行下面的操作。此外，也可以使用这个命令：“`echo | gcc -dM -E -`”。


详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#index-dM-908)

## 贡献者

nanxiao

