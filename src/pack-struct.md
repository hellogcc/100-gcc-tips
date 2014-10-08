# 改变结构体成员的字节对齐

## 例子  
	#include <stdio.h>

	typedef struct
	{
			char a;
			int b;
	} ST_A;

	int main(void)
	{
			printf("sizeof(ST_A)=%ld\n",sizeof(ST_A));
	}
## 技巧
在上面的程序里，`ST_A`结构体的内存布局默认是这样的：
<table>
   <tr>
      <td>Offset</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
   </tr>
   <tr>
      <td>0</td>
      <td>a</td>
      <td>填充字节</td>
      <td>填充字节</td>
	  <td>填充字节</td>
   </tr>
   <tr>
      <td>4</td>
      <td>b</td>
      <td>b</td>
      <td>b</td>
	  <td>b</td>
   </tr>
</table>

编译执行，结果如下：	

    root@ubuntu:~$ gcc -g -o a a.c
	root@ubuntu:~$ ./a
	sizeof(ST_A)=8



使用gcc的"`-fpack-struct[=n]`"选项（“`n`”需要为`2`的倍数）可以改变成员的地址对齐。例如指定“`n=2`”时，将标明结构体成员的最大对齐地址为2。这样`ST_A`结构体中的成员`b`的地址将不再按照`4`字节对齐，内存布局变为：
<table>
   <tr>
      <td>Offset</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
   </tr>
   <tr>
      <td>0</td>
      <td>a</td>
      <td>填充字节</td>
      <td>b</td>
	  <td>b</td>
   </tr>
   <tr>
      <td>4</td>
      <td>b</td>
      <td>b</td>
      <td></td>
	  <td></td>
   </tr>
</table>
编译执行，结果如下：	

    root@ubuntu:~$ gcc -g -fpack-struct=2 -o a a.c
	root@ubuntu:~$ ./a
	sizeof(ST_A)=6

当不指定“`n`”时，将没有填充字节，所有成员将一个挨着一个排在一起：
<table>
   <tr>
      <td>Offset</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
      <td>1byte</td>
   </tr>
   <tr>
      <td>0</td>
      <td>a</td>
      <td>b</td>
      <td>b</td>
	  <td>b</td>
   </tr>
   <tr>
      <td>4</td>
      <td>b</td>
      <td></td>
      <td></td>
	  <td></td>
   </tr>
</table>
编译执行，结果如下：	

    root@ubuntu:~$ gcc -g -fpack-struct -o a a.c
	root@ubuntu:~$ ./a
	sizeof(ST_A)=5
由于这个编译选项会导致ABI(Application Binary Interface)的改变，所以使用时一定要谨慎。
详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Code-Gen-Options.html)
## 贡献者
nanxiao