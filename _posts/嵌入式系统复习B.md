---
title: 嵌入式系统复习B
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
mathjax: false
katex: false
hide: false
abbrlink: a78ce156
date: 2020-08-27 16:10:46
copyright_author: 惠雨&&刘学诚
copyright_author_href:
copyright_url:
copyright_info:
top_img: 
sticky: 80
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

### 嵌入式系统特点

{% hideToggle 嵌入式系统特点 %}

8大特点

- 专用的计算机系统
- 对环境的要求特殊
- 必须是能够满足对系统控制要求的计算机系统
- 是集计算机技术与各行业与一体的集成系统
- 具有较强的生命周期
- 软件固体在非易失性存储器中
- 实时性要求
- 需专用开发环境和开发工具进行设计

{% endhideToggle %}

### `ARM9`系列微处理器的特点

{% note info %}
`ARM9`采用`5`级流水线技术，采用哈佛体系结构(程序存储器与数据存储器分开独立编址)

| 时间片 |   1   |   2   |   3   |   4   |   5   |   6   |   7   |   8   |   9   |
| :----: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 指令1  | 取指  | 译码  | 执行  | 缓冲  | 回写  |       |       |       |
| 指令2  |       | 取指  | 译码  | 执行  | 缓冲  | 回写  |       |       |       |
| 指令3  |       |       | 取指  | 译码  | 执行  | 缓冲  | 回写  |       |       |
| 指令4  |       |       |       | 取指  | 译码  | 执行  | 缓冲  | 回写  |       |
| 指令5  |       |       |       |       | 取指  | 译码  | 执行  | 缓冲  | 回写  |

{% endnote %}

### `R13` `R14` `R15`寄存器的用法、`37`个寄存器分类、区别用法

{% note info %}

`R13`：SP(堆栈指针) 用于保护程序现场

`R14`：LR(子程序链接寄存器) 保存子程序返回地址;设置为异常返回地址

`R15`：PC(程序计数器)

> 在`ARM`所有工作模式下,各模式共享的寄存器是`R0` ~ `R7`
> 两个状态寄存器:其中`SPSR` 当异常发生时,用于保存`CPSR`的当前值,从异常退出时用来恢复`CPSR`
> `ARM`工作模式中具有独立的`R8` ~ `R12`寄存器的是`FIQ`

![寄存器](https://i.loli.net/2020/09/03/3SPZuKQUwVYbepX.jpg)

{% endnote %}

### `ARM`通用寄存器与工作模式的对应关系

|      工作模式       |                    功能说明                    |                      可访问的寄存器                      | CPSR[M4:M0] |
| :-----------------: | :--------------------------------------------: | :------------------------------------------------------: | :---------: |
|   用户模式`User`    |              程序正常执行工作模式              |               `PC(R15)`,`R14`~`R0`,`CPSR`                |   `10000`   |
|  快速中断模式`FIQ`  |    处理高速中断，用于高速数据传输或通道处理    |  `PC`,`R14_fiq`～`R8_fiq`,`R7`～`R0`,`CPSR`,`SPSR_fiq`   |   `10001`   |
|  外部中断模式`IRQ`  |                用于普通中断处理                | `PC`,`R14_irq`～`R13_irq`,`R12`～`R0`, `CPSR`,`SPSR_irq` |   `10010`   |
|    管理模式`SVC`    |      操作系统的保护模式，处理软中断`SWI`       | `PC`,`R14_svc`～`R13_svc`,`R12`～`R0`, `CPSR`,`SPSR_svc` |   `10011`   |
|    中止模式`ABT`    |   处理存储器故障，实现虚拟存储器和存储器保护   | `PC`,`R14_abt`～`R13_abt`,`R12`～`R0`, `CPSR`,`SPSR_abt` |   `10111`   |
| 未定义指令模式`UND` | 处理未定义的指令陷井，用于支持硬件协处理器仿真 | `PC`,`R14_und`～`R13_und`,`R12`～`R0`,`CPSR`,`SPSR_und`  |   `11011`   |
|    系统模式`SYS`    |            运行特权级的操作系统任务            |                 `PC`,`R14`～`R0`,`CPSR`                  |   `11111`   |

### 数据寻址方式

{% hideToggle 数据寻址方式 %}

- **寄存器移位寻址**
  - ![寄存器移位寻址1](https://i.loli.net/2020/09/02/pTBCcbAOiMr23ly.png)
  - ![寄存器移位寻址2](https://i.loli.net/2020/09/02/Qlq41uh7t3oRCST.png)

- **基址加变址寻址**
  - ![寄存器移位寻址1](https://i.loli.net/2020/09/02/pP4TjUc1Othy3zC.png)
  - ![寄存器移位寻址2](https://i.loli.net/2020/09/02/MqA5m6KVv7Dfgh4.png)

- **堆栈寻址**
  - ![堆栈寻址1](https://i.loli.net/2020/09/02/CseTlu8dEX1xfZa.png)
  - ![堆栈寻址2](https://i.loli.net/2020/09/02/GZQkEyzDBwnSXeL.png)

{% endhideToggle %}

### `ARM`寻址空间大小

> **`4G`=`4086M`=`4184064K`**

### `ATPCS`中寄存器组`R0~R3`的使用

{% note info %}
`R0` ~ `R3`用作传入函数参数、传出函数返回值.在子程序调用之间,可以将`R0` ~ `R3`用于任何用途.被调用函数在返回之前不必恢复`R0` ~ `R3`.如果调用函数需要再次使用`R0` ~ `R3`内容,则它必须保留这些内容.

> 混合编程中用来传递参数

{% endnote %}

### `uCOS-II`中任务的五种状态

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

### `uCOS-II`操作系统内核所采用的优先级位图算法的原理及其好处

![关系](https://i.loli.net/2020/08/28/wa6xD9X8bIYLpST.png)

{% note info %}
好处:加快访问任务就续表的速度
{% endnote %}

## Part Two

### `uCOS`中时钟节拍的概念及其作用

{% hideToggle 概念 %}
`uCOS-II`与大多数计算机系统一样，用硬件定时器产生周期为ms级的周期性中断来实现系统时钟，最小的时钟单位就是两次中断之间相间隔的时间，这个最小时钟单位叫做时钟节拍。(`Time Tick`)

硬件定时器以时钟节拍为周期定时地产生中断，该中断地中断服务程序叫做`OSTickISR()`。中断服务程序通过调用函数`OSTimeTick()`来完成系统在每个时钟节拍时需要做的工作。

```C
void OSTickISR(void)
{
  //保存CPU寄存器
  //调用OSIntEnter(); 记录中断嵌套层数
  if(OSIntNesting == 1)
  {
    OSTCBCur->OSTCBStkPtr = SP; //保存堆栈指针
  }
  //调用OSTimeTick(); 节拍处理
  //清除中断
  //开中断
  //调用OSIntExit();  中断嵌套层数减一
  //恢复CPU寄存器;
  //中断返回;
}

void OSTimeTick(void)
{
  ......
  OSTimeTickHook();
  ......
  OSTime++; //记录节拍书
  ......
  if(OSRuning == TRUE)
  {
    ptcb = OSTCBList;
    while(ptcb->OSTCBPrio != OS_IDLE_PRIO)
    {
      OS_ENTER_CRITICAL();
      if(ptcb->OSTCBDly != 0)
      {
        if(--ptcb->OSTCB == 0)  //任务的延时时间减一
        {
          if((ptcb->OSTCBStat & OS_STAT_SUSPEND) == OS_STAT_RDY)
          {
            OSRdyGrp |= ptcb->OSTCBBitY;
            OSRdyTbl[ptcb->OSTCBY] |= ptcb->OSTCBBitX;
          }
          else
          {
            ptcb->OSTCBDly = 1;
          }
        }
      }
      ptcb = ptcb->OSTCBNext;
      OS_EXIT_CRITICAL();
    }
  }
}
```

{% note info %}
`OSTimeTick()`的任务就是在每个时钟节拍了解每个任务的延时状态，使其中已经到了延时时限的非挂起任务进入就绪状态。
{% endnote %}

{% endhideToggle %}

### `S3C2410`存储器的特点

{% note info %}
- 可通过软件选择大小端
- 地址空间:每个`Bank` `128Mbytes`(共`1GB`)
- 除`bank0(16/32-bit)`外,所有的`Bank`都可以通过编程选择总线宽度=(`8/16/32-bit`)
- 一共`8`个`banks`
  - `6`个`Bank`用于控制`ROM`,`SRAM`,etc.
  - 剩余的两个`Bank`用于控制`ROM`,`SRAM`,`SDRAM`,etc.
- `7`个`Bank`固定起始地址
- 最后一个`Bank`可调整起始地址
- 最后两个`Bank`大小可编程
- 所有`Bank`存储周期可编程控制
{% endnote %}

### 读懂`S3C2410`数据手册，结合已给出语句，将串口程序补充完整

```C
/* 包含文件 */
#include "def.h"
#include "2410lib.h"
#include "option.h"
#include "2410addr.h"
#include "interrupt.h"

/********************************************************************
// Function name	: Main
// Description	    : JXARM9-2410 串口通信实验主程序
//                    实现功能：
//                        实现JXRAM9-2410与PC机的串口通讯
//                        JXARM9-2410 UART0 <==> PC COM
// Return type		: void
// Argument         : void
*********************************************************************/
void Main(void)
{
	/* 配置系统时钟 */
    ChangeClockDivider(1,1);          // 1:2:4    
    ChangeMPllValue(0xa1,0x3,0x1);    // FCLK=202.8MHz  
    
    /* 初始化端口 */
    Port_Init();
    
    /* 初始化串口 */
    Uart_Init(0,115200);
    Uart_Select(0);
    
    /* 打印提示信息 */

	PRINTF("\n---UART测试程序---\n");
	PRINTF("\n请将UART0与PC串口进行连接，然后启动超级终端程序(115200, 8, N, 1)\n");
	PRINTF("\n从现在开始您从超级中断发送的字符将被回显在超级终端上\n");
	
    /* 开始回环测试 */
	while(1)
	{
		unsigned char ch = 'a';
		
		ch = Uart_Getch();
		Uart_SendByte(ch);
		if(ch == 0x0d)
			Uart_SendByte(0x0a);
	}
}

```

### `S3C2410`中断实验程序

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


### `IRQ`中断过程

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

{% endhideToggle %}


### `S3C2410`的`GPIO`编程方法

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
