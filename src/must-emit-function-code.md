# 禁止函数被优化掉

## 例子

	#if (GCC_VERSION > 4000)
	#define DEBUG_FUNCTION __attribute__ ((__used__))
	#define DEBUG_VARIABLE __attribute__ ((__used__))
	#else 
	#define DEBUG_FUNCTION
	#define DEBUG_VARIABLE
	#endif
	
	DEBUG_FUNCTION void
	debug_bb (basic_block bb)
	{
	  dump_bb (bb, stderr, 0);
	}

## 技巧

上面的例子是gcc的源码。使用gcc的扩展功能——函数属性`__attribute__ ((__used__))`，可以指定该函数是有用的，不能被优化掉。

详情参见[gcc手册](https://gcc.gnu.org/onlinedocs/gcc/Function-Attributes.html#Function-Attributes)

## 贡献者

xmj

