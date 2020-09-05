---
title: 嵌入式系统复习A
tags:
  - 嵌入式
categories:
  - 嵌入式系统
keywords: '嵌入式,复习'
description: 嵌入式考试复习提纲
comments: true
toc: true
toc_number: true
auto_open: true
highlight_shrink: true
copyright: true
copyright_author: 惠雨&&刘学诚
copyright_author_href: 
copyright_url: 
copyright_info: 
mathjax: false
katex: false
hide: false
abbrlink: 3e85b0ec
date: 2020-08-27 16:10:46
top_img: 
sticky: 90
cover: 
---

# 试卷结构

|    题型    |     分值     |
| :--------: | :----------: |
|   选择题   | 2分✖️10=20分  |
| 判断改错题 |  2分✖️6=12分  |
|   简答题   |  6分✖️3=18分  |
| 程序分析题 | 共四题，40分 |
|   设计题   | 共一题，10分 |

# 考点

## Part One

### 熟悉嵌入式系统的特点、ARM公司

{% hideToggle 嵌入式系统特点 %}

8大特点

- 专用的计算机系统
- 对环境的要求特殊
- 必须是能够满足对系统控制要求的计算机系统
- 是集计算机技术与各行业于一体的集成系统
- 具有较强的生命周期
- 软件固体在非易失性存储器中
- 实时性要求
- 需专用开发环境和开发工具进行设计

{% endhideToggle %}

{% hideToggle ARM公司 %}

- `ARM`(ADVANCED RISC MACHINES)公司成立于`1990`年，目前，`ARM`架构处理器在高性能、低功耗、低成本应用领域占据领先地位
- `ARM`公司是嵌入式`RISC`处理器的知识产权`IP`供应商
- 半导体公司在`ARM`技术的基础上，根据自己公司的产品定位，添加自己的设计，潜入各种外围和处理部件，推出芯片产品形成各种嵌入式微处理器`MPU`或微控制器`MCU`

{% endhideToggle %}

### `CPSR`寄存器的作用、各组成位的含义

{% note warning %}
`CPSR`状态寄存器：

![CPSR](https://i.loli.net/2020/08/27/C29qHt5ezKLW4wh.png)

![CPSR](https://i.loli.net/2020/09/03/Bhz2FjMESbmyiX6.png)

![Bits](https://i.loli.net/2020/09/03/QMhqsCmgtpTd2uk.png)

{% endnote %}

### 常见的嵌入式操作系统

> - uC/OS-II
> - (uC)Linux
> - Palm OS
> - Win CE
> - VxWorks

{% note warning %}
嵌入式操作系统的特点：（注意与前面的嵌入式系统区分）

- 编码体积小
- 面向应用
- 实时性强
- 可移植性好
- 可靠性高
- 专用性强

{% endnote %}

### `R13` `R14` `R15`寄存器的用法、37个寄存器分类、用法区别

{% note info %}

`R13`：SP(堆栈指针) 用于保护程序现场

`R14`：LR(子程序链接寄存器) 保存子程序返回地址;设置为异常返回地址

`R15`：PC(程序计数器) 指向当前指令的下两条指令的地址

> 在`ARM`所有工作模式下,各模式共享的寄存器是`R0` ~ `R7`
> 两个状态寄存器:其中`SPSR` 当异常发生时,用于保存`CPSR`的当前值,从异常退出时用来恢复`CPSR`
> `ARM`工作模式中具有独立的`R8` ~ `R12`寄存器的是`FIQ`

![寄存器](https://i.loli.net/2020/09/03/3SPZuKQUwVYbepX.jpg)

{% endnote %}

### `ARM`通用寄存器与工作模式的对应关系

|      工作模式       |                    功能说明                    |                      可访问的寄存器                      | CPSR[M4:M0] |
| :-----------------: | :--------------------------------------------: | :------------------------------------------------------: | :---------: |
|    中止模式`ABT`    |   处理存储器故障，实现虚拟存储器和存储器保护   | `PC`,`R14_abt`～`R13_abt`,`R12`～`R0`, `CPSR`,`SPSR_abt` |   `10111`   |
|  外部中断模式`IRQ`  |                用于普通中断处理                | `PC`,`R14_irq`～`R13_irq`,`R12`～`R0`, `CPSR`,`SPSR_irq` |   `10010`   |
|  快速中断模式`FIQ`  |    处理高速中断，用于高速数据传输或通道处理    |  `PC`,`R14_fiq`～`R8_fiq`,`R7`～`R0`,`CPSR`,`SPSR_fiq`   |   `10001`   |
| 未定义指令模式`UND` | 处理未定义的指令陷井，用于支持硬件协处理器仿真 | `PC`,`R14_und`～`R13_und`,`R12`～`R0`,`CPSR`,`SPSR_und`  |   `11011`   |
|   用户模式`User`    |              程序正常执行工作模式              |               `PC(R15)`,`R14`~`R0`,`CPSR`                |   `10000`   |
|    管理模式`SVC`    |      操作系统的保护模式，处理软中断`SWI`       | `PC`,`R14_svc`～`R13_svc`,`R12`～`R0`, `CPSR`,`SPSR_svc` |   `10011`   |
|    系统模式`SYS`    |            运行特权级的操作系统任务            |                 `PC`,`R14`～`R0`,`CPSR`                  |   `11111`   |

### `uCOS-II`中的五种状态的含义、区别

- 等待状态
  - 正在运行的任务，需要等待一段时间或需要等待一个事件发生再运行时，该任务就会把CPU的使用权让给别的任务，而使任务进入等待状态
- 睡眠状态
  - 任务在没有被配备任务控制块或被剥夺了任务控制块时的状态叫做任务的睡眠状态
- 就绪状态
  - 任务为任务分配了任务控制块且在任务就绪表中进行了就绪登记，这时任务的状态叫做就绪状态
- 运行状态
  - 处于就绪状态的任务如果经调度器获得了CPU的使用权，则任务就进入了运行状态
- 中断服务状态
  - 一个正在运行的任务一旦响应中断申请就会终止运行而去执行中断服务程序，这时任务的状态叫做中断服务状态

{% note info %}
任务一经创建立即进入就绪状态
{% endnote %}

![](https://i.loli.net/2020/09/03/bHJorcY8fOlDGn9.png)

![五种状态](https://i.loli.net/2020/08/28/tTgmzcvG9xk7W2U.png)

### `uCOS-II`中任务就绪表的作用、任务优先级的定义、区别

{% note info %}
任务就绪表是`uCOS-II`进行任务调度的依据,记录了任务是否处于就绪状态

{% hideToggle 任务就绪表的定义及相关变量 %}

系统中的每个任务都在这个表中占据了一个位置，并用这个位置的状态(1或0)来表示任务是否处于就绪状态，这个表就叫做任务就绪状态表，简称任务就绪表。

任务就绪表就是一个二维数组`OSRdyTbl[]`

![任务就绪表](https://i.loli.net/2020/08/28/8IzVGwKZCfoNaqA.png)

为了加快访问任务就绪表的速度，系统定义了一个变量`OSRdyGrp`来表明就绪表每行中是否存在就绪任务

![OSRdyGrp](https://i.loli.net/2020/08/28/ivrjEo5yuCcWsg8.png)

两者关系示意图

![关系](https://i.loli.net/2020/08/28/wa6xD9X8bIYLpST.png)

{% endhideToggle %}

{% endnote %}

{% note info %}

任务优先级`prio`

![prio](https://i.loli.net/2020/08/28/uN7hlD1MiVSvHca.png)

{% hideToggle 任务优先级使用 %}

如图：把`prio`为`29`的任务置为就绪状态

![事例](https://i.loli.net/2020/08/28/QLZKUghPFW3zfMA.png)

代码实现：

```C
//在程序中，可以使用类似下面的代码把优先级别为`prio`的任务置为就绪状态
OSRdyGrp |= OSMapTbl[prio >> 3];
OSRdyTbl[prio>>3] |= OSMapTbl[prio & 0x07];

//如果要使一个优先级别为`prio`的任务脱离就绪状态则可使用如下类似的代码：
if((OSRdyTbl[prio >> 3] &= ~OSMapTbl[prio & 0x07]) == 0)
  OSRdyGrp &= ~OSMapTbl[prio >> 3];
```

下图是在就绪表中查找最高优先级别任务的过程：

![查找最高优先级过程](https://i.loli.net/2020/08/28/gTWn3i6OjILbEx8.png)

代码实现：

```C
//从任务就绪表中获取优先级别最高的就绪任务可用如下类似的代码：
y = OSUnMapTal[OSRdyGrp]; //D5、D4、D3位
x = OSUnMapTal[OSRdyTbl[y]];  //D2、D1、D0位
prio = (y<<3) + x;  //优先级别

//或者也可以使用以下代码：
y = OSUnMapTbl[OSRdyGrp];
prio = (INT8U)((y << 3) + OSUnMapTbl[OSRdyTbl[y]];
```

优先级判定表`OSUnMapTbl[256](os_core.c)`

![优先级判断表](https://i.loli.net/2020/08/28/kvQVd7qUlSeM4pc.png)

{% endhideToggle %}

{% endnote %}

### 处理器中`CACHE(高速缓冲存储器)`的含义、作用

> 含义:小型,快速的存储器
> 作用:存放最近一段时间微处理器使用最多的程序代码和数据

*需要进行数据读取操作时,微处理器尽可能地从`Cache`中读取数据,而不是从内存中读取,这样就大大改善了系统的性能*

### 嵌入式系统与计算机系统的关系

{% note info %}
嵌入式系统是计算机系统,应用在专业领域
{% endnote %}

### `ARM7\ARM9`以及冯诺依曼结构与哈佛结构

> `ARM7`使用的是冯诺依曼结构
> `ARM9`使用的是哈佛结构

{% hideToggle 冯诺依曼结构与哈佛结构 %}

{% note info %}
冯诺依曼结构：

使用冯诺依曼结构的处理器使用同一个存储器，即程序和数据共用一个存储器，由一个总线传输。

![冯诺依曼](https://i.loli.net/2020/08/27/zbi9g26LBZWmM3r.png)

{% endnote %}

{% note info %}
哈佛结构：

与冯诺依曼结构相比，哈佛结构处理器有两个明显的特点：

- 使用两个独立的存储器模块，分别存储指令和数据，每个存储模块都不允许指令和数据并存
- 使用独立的两条总线，分别作为`CPU`与每个存储器之间的专用通信路径，而这两条总线之间毫无关联

{% endnote %}

{% endhideToggle %}

### `ARM`处理器从异常返回的步骤

{% note info %}
异常返回步骤：

- 恢复原来被保护的用户寄存器
- 将`SPSR_mode`寄存器值复制到`CPSR`中，使得`CPSR`从相应的`SPSR`中恢复，以恢复被中断的程序工作状态
- 根据异常的类型将`PC`的值恢复成断点地址，以执行用户原来运行着的程序
- 清除`CPSR`中的中断(`I=0`和`F=0`)，开放外部中断和快速中断

{% endnote %}

{% note warning %}
*补充异常响应过程*：

- 保护状态标志
  - 将`CPSR`的值保存到将要执行的异常中断对应的各自`SPSR`中(保护目的，并不是传统的压入堆栈的方法)
- 设置工作模式并禁止中断
  - 设置工作模式(M4:M0的五个位)
  - 设置禁止中断(`I=1`禁止`IRQ`中断,`F=1`禁止`FIQ`中断)
- 保护断点地址
  - 将引起异常指令的下一条地址（断点地址）保存到新的异常工作模式的`R14`中
- 转入中断服务程序入口地址
  - 给程序计数器`PC`强制赋值，使转入向量地址，以便执行相应的处理程序

{% endnote %}

### `uCOS-II`中空闲任务的概念

{% note info %}

`uCOS-II`正常启动,自动建立一个空闲任务,空闲任务的优先级最低,其值是`OS_LOWEST_PRIO`。在没有其他任务进入就绪状态的情况下，空闲任务会一直运行。

空闲任务运行的过程中不断给32位计数器`OSLdleCtr`加`1`,用于确定空闲任务消耗的`CPU`时间.在对计数器加`1`的过程中,为了防止空闲任务被中断服务子程序害怕任务优先级任务打断,加`1`要作为临界端代码.

{% endnote %}

### `ARM`汇编中符号`LDR`的两种用法

{% hideToggle 内存访问指令 %}

`LDR`指令可以从内存中读取数据到寄存器中.

```assembly
ldr r1, [r2, #4] /*将地址为r2+4的内存单元数据读取到r1中*/
ldr r1, [r2], #4  /*将地址为r2的内存单元数据读取到r1中,然后把r2=r2+4*/
```

{% endhideToggle %}

{% hideToggle 地址读取伪指令 %}

> `ldr`伪指令不是真实存在的指令,编译器会把它扩展成真正的指令
> 如果该常数能用"立即数"表示,则使用`MOV`指令,否则编译时将该常数保存在某个位置,使用内存读取指令把它读出来.

```assembly
LDR{cond} register,=expr
;其中,expr可以是一个32位常数,也可以是程序代码中的标号

;实际应用
LDR R1,=0xAABBCCDD  ;将立即数0xAABBCCDD放入R1中
LDR R0,=place ;将标号place地址放入R0中
...
LTORG ;声明文字池
...
```

{% endhideToggle %}

## Part Two

### `ARM`异常向量表

```assembly
@=========================================
@ Exception entry
@=========================================

        b	ResetHandler  
	b	HandlerUndef	@handler for Undefined mode
	b	HandlerSWI	    @handler for SWI interrupt
	b	HandlerPabort	@handler for PAbort
	b	HandlerDabort	@handler for DAbort
	b	.		        @reserved
	b	HandlerIRQ	    @handler for IRQ interrupt 
	b	HandlerFIQ	    @handler for FIQ interrupt
```

|   异常类型   |                                   具体含义                                   |
| :----------: | :--------------------------------------------------------------------------: |
|     复位     |           复位电平有效时,产生复位异常,程序跳转到复位处理程序处执行           |
|  未定义指令  |                   遇到不能处理的指令时,产生未定义指令异常                    |
|   软件中断   |             执行SWI指令产生,用于用户模式下的程序调用特权操作指令             |
| 指令预取中止 |  处理器预取指令的地址不存在,或该地址不允许当前指令访问,产生指令预取终止异常  |
|   数据终止   | 处理器数据访问指令的地址不存在,或该地址不允许当前指令访问时,产生数据终止异常 |
|     IRQ      |               外部中断请求有效,且CPSR中的I位为0时,产生IRQ异常                |
|     FIQ      |             快速中断请求引脚有效,且CPSR中的F位为0时,产生FIQ异常              |

![异常向量表](https://i.loli.net/2020/09/02/kwMzfsAmLN6pIlZ.png)

### `S3C2410`中断实验

{% hideToggle uCOS-II系统响应中断的过程 %}
过程：

- 系统接受到中断请求后，这时如果CPU处于中断允许状态，系统就会中止正在运行的当前任务，而按照中断向量的指向转而去运行中断服务子程序
- 当中断服务子程序运行结束后，系统将会根据情况返回到被中止的任务继续运行或者转向运行另一个=具有更高优先级别的就绪任务

{% note warning %}
中断服务子程序运行结束后，系统回根据情况进行一次任务调度去运行你优先级别更高的就绪任务，并不是一定要接续运行被中断的任务。
{% endnote %}

中断响应过程示意图：

![中断响应过程](https://i.loli.net/2020/08/28/pJ2nx8wai4qFSVA.png)

![中断响应过程](https://i.loli.net/2020/09/02/bIQYpCDNXPLWR3U.png)

```C
/* 包含文件 */
#include "def. ph"
#include "2410lib.h"
#include "option.h"
#include "2410addr.h"
#include "interrupt.h"

/* functions */
void eint2_isr(void) __attribute__ ((interrupt("IRQ")));;
void eint3_isr(void) __attribute__ ((interrupt("IRQ")));;
void delay();

/* variables */
int dither_count2 = 0;
int dither_count3 = 0;
static int nLed = 0;

/*****************************************************************************
// Function name	: Main
// Description	    : JXARM9-2410 中断实验主程序
//                    完成功能:
//                        外部中断按键引发中断
// Return type		: void
// Argument         : void
*****************************************************************************/
void Main(void)
{
	/* 配置系统时钟 */
    ChangeClockDivider(1,1);          // 1:2:4    
    ChangeMPllValue(0xa1,0x3,0x1);    // FCLK=202.8MHz  
	
	/* 中断初始化 */
    Isr_Init();
    /* 初始化端口 */
    Port_Init();
    
    /* 初始化串口 */
    Uart_Init(0,115200);  //2410内部时钟信号PCLK,波特率115200
    Uart_Select(0); //选择通道0

    /* 打印提示信息 */
	PRINTF("\n---外部中断测试程序---\n");
	PRINTF("\n请将UART0与PC串口进行连接，然后启动超级终端程序(115200, 8, N, 1)\n");
	PRINTF("\n外部中断测试开始\n");

	/* 请求中断 */
	Irq_Request(IRQ_EINT2, eint2_isr);
	Irq_Request(IRQ_EINT3, eint3_isr);

    /* 使能中断 */
    Irq_Enable(IRQ_EINT2);
    Irq_Enable(IRQ_EINT3);
       	
    dither_count2 = 0;
    dither_count3 = 0;
    while(1)
    {
    	delay();
    	dither_count2++;
    	dither_count3++;
    }
}

/*****************************************************************************
// Function name	: eint2_isr
// Description	    : EINT2中断处理程序
// Return type		: int
// Argument         : void
*****************************************************************************/
void eint2_isr(void)
{
	Irq_Clear(IRQ_EINT2);         

    if(dither_count2 > 5) 
    {
	   	dither_count2 = 0;
	
		Led_Display(nLed);

		nLed = (nLed ^ 0x01);
	}
}

/*****************************************************************************
// Function name	: eint3_isr
// Description	    : EINT3中断处理程序
// Return type		: int
// Argument         : void
*****************************************************************************/
void eint3_isr(void)
{
	Irq_Clear(IRQ_EINT3);        

    if(dither_count3 > 5) 
    {
	   	dither_count3 = 0;
	
		Led_Display(nLed);

		nLed = (nLed ^ 0x02);
	}
}

void delay()
{
	int index = 0; 
	
	for ( index = 0 ; index < 20000; index++);
}


```

{% endhideToggle %}

### 基于`uCOS-II`的最基本`main()`函数

```C
void main(void){
  ......
  OSInit(); //对uCOS-II进行初始化
  ......
  OSTaskCreate(TaskStart,......); //创建任务TaskStart
  OSStart();  //开始多任务调度
}

void TaskStart(void *pdata){
  ......  //在这个位置安装并启动uCOS-II的时钟
  OSStatInit(); //初始化统计任务
  ......  //在这个位置创建其他任务
  for(;;){
    //起始任务TaskStart的代码
  }
}
```

### `uCOS-II`的邮箱功能

{% hideToggle 消息邮箱及其操作 %}

{% note info %}
如果把数据缓冲区的指针赋给一个事件控制块的成员`OSEventPrt`，同时使事件控制块的成员`OSEventType`为常数`OS_EVENT_TYPE_MBOX`，则该事件控制块就叫做消息信箱，消息信箱是在两个需要通信的任务之间通过传递数据缓冲区指针的方法来通信的。

消息信箱的结构：

![消息信箱](https://i.loli.net/2020/08/28/e2Zqw7TO1WHxrLp.png)

> 创建消息信箱需要调用函数`OSMboxCreate()`，这个函数的原型为：
>
 ```C
 OS_EVENT *OSMboxCreate(
   void *msg //消息指针
  )；
 ```

> 函数中的参数`msg`为消息的指针，函数的返回值为消息信箱的指针。
> 调用函数`OSMboxCreate()`需先定义`msg`的初始值。
> 在一般情况下，这个初始值为`NULL`；但也可以事先定义一个邮箱，然后把这个邮箱的指针作为参数传递到函数`OSMboxCreate()`中，使之一开始就指向一个邮箱。
>

> 任务可以通过调用函数`OSMbixPost()`向消息邮箱发送消息，这个函数的原型为：

```C
INIT8U OSMboxPost(
  OS_EVENT *pevent, //消息信箱指针
  void *msg //消息指针
);
```

> 当一个任务请求邮箱时需要调用函数`OSMboxPend()`，这个函数的主要作用就是查看邮箱指针`OSEventPrt`是否为`NULL`，如果不是`NULL`就把邮箱中的消息指针返回给调用函数的任务，同时用`OS_NO_ERR`通过函数的参数`err`通知任务获取消息成功；如果邮箱指针`OSEventPtr`是`NULL`，则使任务进入等待状态，并引发一次任务调度。
> 函数`OSMboxPend()`的原型为：

```C
void *OSMboxPend(
  OS_Event *pevent, //请求消息邮箱指针
  INT16U timeout, //等待时限
  INT8U *err  //错误信息
);

```

{% endnote %}

{% endhideToggle %}


### `S3C2410X`的`GPIO`编程方法

{% hideToggle GPIO编程方法 %}
![GPIOF](https://i.loli.net/2020/09/02/8jOtYhe6JCfXoxK.png)

```C
#define rGPFCON *(*unsigned int)0x56000050
#define rGPFDAT *(*unsigned int)0x56000054
#define rGPFUP *(*unsigned int)0x56000058

void led_on_off(void ){
  int i;
  rGPFCON = 0x5500;
  while(1){
    rGPFDAT = 0x00;
    for(i = 0;i < 100000;i++);
    rGPFDAT = 0xF0;
    for(i = 0;i < 100000;i++);
  }
}
```

{% endhideToggle %}
