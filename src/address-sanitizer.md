# 利用Address Sanitizer工具检查内存访问错误

## 例子  
	a.c:
	#include <stdio.h>

	int main(void) {
	        // your code goes here
	        int a[3] = {0};
	        a[3] = 1;
	
	        printf("%d\n", a[3]);
	        return 0;
	}

	b.c:
	#include <stdio.h>
	#include <malloc.h>
	
	int main(void) {
	        int *p = NULL;
	
	        p = malloc(10 * sizeof(int));
	        free(p);
	        *p = 3;
	        return 0;
	}


## 技巧
gcc从`4.8`版本起，集成了`Address Sanitizer`工具，可以用来检查内存访问的错误（编译时指定“`-fsanitize=address`”）。以上面`a.c`程序为例：  

	gcc -fsanitize=address -g -o a a.c
执行`a`程序：  

	[root@localhost nan]# ./a
	=================================================================
	==539==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7fff3a152c9c at pc 0x4009b6 bp 0x7fff3a152c60 sp 0x7fff3a152c58
	WRITE of size 4 at 0x7fff3a152c9c thread T0
	    #0 0x4009b5 in main /home/nan/a.c:6
	    #1 0x34e421ed1c in __libc_start_main (/lib64/libc.so.6+0x34e421ed1c)
	    #2 0x4007b8 (/home/nan/a+0x4007b8)
	
	Address 0x7fff3a152c9c is located in stack of thread T0 at offset 44 in frame
	    #0 0x400907 in main /home/nan/a.c:3
	
	  This frame has 1 object(s):
	    [32, 44) 'a' <== Memory access at offset 44 overflows this variable
	HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
	      (longjmp and C++ exceptions *are* supported)
	SUMMARY: AddressSanitizer: stack-buffer-overflow /home/nan/a.c:6 main
	Shadow bytes around the buggy address:
	  0x100067422540: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100067422550: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100067422560: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100067422570: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x100067422580: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 f1 f1
	=>0x100067422590: f1 f1 00[04]f4 f4 f3 f3 f3 f3 00 00 00 00 00 00
	  0x1000674225a0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000674225b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000674225c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000674225d0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	  0x1000674225e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
	Shadow byte legend (one shadow byte represents 8 application bytes):
	  Addressable:           00
	  Partially addressable: 01 02 03 04 05 06 07
	  Heap left redzone:       fa
	  Heap right redzone:      fb
	  Freed heap region:       fd
	  Stack left redzone:      f1
	  Stack mid redzone:       f2
	  Stack right redzone:     f3
	  Stack partial redzone:   f4
	  Stack after return:      f5
	  Stack use after scope:   f8
	  Global redzone:          f9
	  Global init order:       f6
	  Poisoned by user:        f7
	  Contiguous container OOB:fc
	  ASan internal:           fe
	==539==ABORTING
可以看到，执行程序时检测出了`a`数组的越界访问（`a[3] = 1`）。

再看一下`b`程序：  

	gcc -fsanitize=address -g -o b b.c
执行`b`程序：  

	[root@localhost nan]# ./b
	=================================================================
	==1951==ERROR: AddressSanitizer: heap-use-after-free on address 0x60400000dfd0 at pc 0x4007f9 bp 0x7fff34277bb0 sp 0x7fff34277ba8
	WRITE of size 4 at 0x60400000dfd0 thread T0
	    #0 0x4007f8 in main /home/nan/b.c:9
	    #1 0x34e421ed1c in __libc_start_main (/lib64/libc.so.6+0x34e421ed1c)
	    #2 0x400658 (/home/nan/b+0x400658)
	
	0x60400000dfd0 is located 0 bytes inside of 40-byte region [0x60400000dfd0,0x60400000dff8)
	freed by thread T0 here:
	    #0 0x7fbbb7a7d057 in __interceptor_free /opt/gcc-4.9.2/src/gcc-4.9.2/libsanitizer/asan/asan_malloc_linux.cc:62
	    #1 0x4007c1 in main /home/nan/b.c:8
	    #2 0x34e421ed1c in __libc_start_main (/lib64/libc.so.6+0x34e421ed1c)
	
	previously allocated by thread T0 here:
	    #0 0x7fbbb7a7d26f in __interceptor_malloc /opt/gcc-4.9.2/src/gcc-4.9.2/libsanitizer/asan/asan_malloc_linux.cc:72
	    #1 0x4007b1 in main /home/nan/b.c:7
	    #2 0x34e421ed1c in __libc_start_main (/lib64/libc.so.6+0x34e421ed1c)
	
	SUMMARY: AddressSanitizer: heap-use-after-free /home/nan/b.c:9 main
	Shadow bytes around the buggy address:
	  0x0c087fff9ba0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9bb0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9bc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9bd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9be0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	=>0x0c087fff9bf0: fa fa fa fa fa fa fa fa fa fa[fd]fd fd fd fd fa
	  0x0c087fff9c00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9c10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9c20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9c30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c087fff9c40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	Shadow byte legend (one shadow byte represents 8 application bytes):
	  Addressable:           00
	  Partially addressable: 01 02 03 04 05 06 07
	  Heap left redzone:       fa
	  Heap right redzone:      fb
	  Freed heap region:       fd
	  Stack left redzone:      f1
	  Stack mid redzone:       f2
	  Stack right redzone:     f3
	  Stack partial redzone:   f4
	  Stack after return:      f5
	  Stack use after scope:   f8
	  Global redzone:          f9
	  Global init order:       f6
	  Poisoned by user:        f7
	  Contiguous container OOB:fc
	  ASan internal:           fe
	==1951==ABORTING
执行程序时检测出了访问释放内存的错误（`*p = 3`）。  
详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc-4.9.2/gcc/Debugging-Options.html#index-fsanitize_003daddress-593)
## 贡献者
nanxiao