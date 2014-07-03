# 在命令行中预定义宏

## 例子

	#include <stdio.h>
	
	int main (void)
	{
	  int i, sum;
	
	  for (i = 1, sum = 0; i <= 10; i++)
	    {
	      sum += i;
	    #ifdef DEBUG
	      printf ("sum += %d is %d\n", i, sum);
	    #endif
	    }
	  printf ("total sum is %d\n", sum);
	
	  return 0;
	}

## 技巧

使用`-D`选项可以在命令行中预定义一个宏，比如：

	$ gcc -D DEBUG macro.c

中间可以没有空格：

	$ gcc -DDEBUG macro.c

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Preprocessor-Options.html#Preprocessor-Options)

## 贡献者

xmj

