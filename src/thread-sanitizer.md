# 利用Thread Sanitizer工具检查数据竞争的问题

## 例子  
	#include <pthread.h>
	int Global;
	void *Thread1(void *x) {
	  Global = 42;
	  return x;
	}
	int main(void) {
	  pthread_t t;
	  pthread_create(&t, NULL, Thread1, NULL);
	  Global = 43;
	  pthread_join(t, NULL);
	  return Global;
	}



## 技巧
gcc从`4.8`版本起，集成了`Address Sanitizer`工具，可以用来检查数据竞争的问题（编译时指定“`-fsanitize=thread -fPIE -pie`”）。以上面程序为例：  

	gcc -fsanitize=thread -fPIE -pie -g -o a a.c -lpthread

执行`a`程序：  

	[root@localhost nan]# ./a
	==================
	WARNING: ThreadSanitizer: data race (pid=14545)
	  Write of size 4 at 0x7f055b4802b0 by thread T1:
		#0 Thread1 /home/nan/a.c:4 (a+0x000000000a87)

	  Previous write of size 4 at 0x7f055b4802b0 by main thread:
		#0 main /home/nan/a.c:10 (a+0x000000000ae8)

	  Location is global 'Global' of size 4 at 0x7f055b4802b0 (a+0x0000002012b0)

	  Thread T1 (tid=14547, running) created by main thread at:
		#0 pthread_create /opt/gcc-4.9.2/src/gcc-4.9.2/libsanitizer/tsan/tsan_interceptors.cc:877 (libtsan.so.0+0x00000004aa83)
		#1 main /home/nan/a.c:9 (a+0x000000000ad9)

	SUMMARY: ThreadSanitizer: data race /home/nan/a.c:4 Thread1
	==================
	ThreadSanitizer: reported 1 warnings

可以看到，执行程序时检测出了对`Global`变量的竞争访问。  
详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc-4.9.2/gcc/Debugging-Options.html#index-fsanitize_003dthread-595)
## 贡献者
nanxiao