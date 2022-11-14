本文主要介绍STM32F103系列芯片的[地址映射](https://so.csdn.net/so/search?q=%E5%9C%B0%E5%9D%80%E6%98%A0%E5%B0%84&spm=1001.2101.3001.7020)和寄存器映射原理，GPIO端口的初始化设置步骤、利用C语言编程和寄存器点亮流水灯以及stm32CubeMX+Keil使用HAL库点灯

**目录**

[一、STM32F103系列芯片的地址映射和寄存器映射原理](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t0)

[1、什么是寄存器](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t1)

[2、地址映射和寄存器映射原理](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t2)

[3、GPIO端口的初始化设置三步骤（时钟配置、输入输出模式设置、最大速率设置）](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t3)

[（1）GPIO初始化步骤](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t4)

[（2）GPIO简介](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t5)

[（3）GPIO模式](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t6)

[编辑](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t7)

[4、为什么要先配置时钟，再配置GPIO？](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t8)

[二、以寄存器方式点亮流水灯](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t9)

[1、题目要求](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t10)

[2、建立工程文件](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t11)

[3、使用寄存器点亮LED灯——代码部分](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t12)

[（1）硬件连接设计](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t13)

[（2）代开之前建立的工程，编写代码](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t14)

[（3）硬件连接](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t15)

 [(4) 烧录：STM32F103C8T6与PC端连接](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t16)

 [四、使用寄存器点亮LED灯——电路部分](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t17)

 [五、stm32CubeMX+Keil使用HAL库点亮流水灯](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t18)

[1、题目要求](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t19)

[2、STM32CubeMX简介与下载](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t20)

[3、利用CubeMX新建工程点亮LED灯](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t21)

[（1）前期准备](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t22)

[（2）新建工程](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t23)

 [（3）界面介绍](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t24)

[（4）配置引脚](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t25)

[（5）配置工程环境](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t26)

[4、编译及烧录](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t27)

 [五、总结](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t28)

[六、参考文献](https://blog.csdn.net/Xicun1984/article/details/127308398?spm=1001.2014.3001.5502#t29)

___

___

## 一、STM32F103系列芯片的地址映射和[寄存器](https://so.csdn.net/so/search?q=%E5%AF%84%E5%AD%98%E5%99%A8&spm=1001.2101.3001.7020)映射原理

## 1、什么是寄存器

       根据百度百科介绍，寄存器是中央处理器内的组成部分。  寄存器是有限存贮容量的高速存贮部件，它们可用来暂存指令、数据和地址。

       这是比较专业化的解释，理解起来比较难，简单来说，寄存器就是存放东西的一个空间器物。寄存器可能存放的是指令、数据或地址。  
　　存放数据的寄存器是最好理解的，如果你需要读取一个数据，直接到这个寄存器所在的地方来问问他，数据是多少就行了。问寄存器这个动作，叫做访问寄存器。不同的数据会存放在不同的寄存器，例如引脚PA2与PB8的高低电平数据（1或0）肯定放在不同的寄存器里，那么怎么区分不同的寄存器呢？通过地址，不同的寄存器有不同的地址，就像老张行李寄存处在101号店铺，老王行李寄存处在258号店铺。  
　　指令、地址寄存器与数据寄存器类似，里边存放的都是0和1，毕竟单片机也只认识机器码，机器码都是0或1，只是特别的规定下，数据寄存器里面存放的0和1表示数据，指令寄存器里存放的表示指令。

## 2、地址映射和寄存器映射原理

（1）地址映射：由百度词条可知为了保证CPU执行指令时可正确访问存储单元，需将用户程序中的逻辑地址转换为运行时由机器直接寻址的物理地址，这一过程称为地址映射。  
（2）寄存器映射：在存储器的区域单元中，每一个单元对应不同的功能，当我们控制这些单元时就可以驱动外设工作。我们可以找到每个单元的起始地址，然后通过 C 语言指针的操作方式来访问这些单元，如果每次都是通过这种地址的方式来访问，不仅不好记忆还容易出错，这时我们可以根据每个单元功能的不同，以功能为名给这个内存单元取一个别名，这个别名就是我们经常说的寄存器，这个给已经分配好地址的有特定功能的内存单元取别名的过程就叫寄存器映射。

## 3、[GPIO](https://so.csdn.net/so/search?q=GPIO&spm=1001.2101.3001.7020)端口的初始化设置三步骤（时钟配置、输入输出模式设置、最大速率设置）

### （1）GPIO初始化步骤

第一步：使能GPIOx口的时钟  
第二步：指明GPIOx口的哪一位，这一位的速度大小以及模式  
第三步：调用GPIOx初始化函数进行初始化  
第四步：调用GPIO-SetBits函数，进行相应位的置位

### （2）GPIO简介

GPIO 是通用输入输出端口的简称，简单来说就是 STM32 可控制的引脚，STM32 芯片的 GPIO 引脚与外部设备连接起来，从而实现与外部通讯、控制以及数据采集的功能。STM32 芯片的 GPIO 被分成很多组，每组有 16 个引脚。

如型号为 STM32F103VET6 型号的芯片有 GPIOA、GPIOB、GPIOC至 GPIOE共 5组 GPIO，芯片一共 100个引脚，其中 GPIO就占了一大部分，所有的 GPIO 引脚都有基本的输入输出功能。

最基本的输出功能是由 STM32 控制引脚输出高、低电平，实现开关控制，如把 GPIO引脚接入到 LED灯，那就可以控制 LED灯的亮灭，引脚接入到继电器或三极管，那就可以通过继电器或三极管控制外部大功率电路的通断。

最基本的输入功能是检测外部输入电平，如把 GPIO 引脚连接到按键，通过电平高低  
区分按键是否被按下。

### （3）GPIO模式

## ![](https://img-blog.csdnimg.cn/489da109ff5c406dad8de9308e0cf5cb.png)

GPIO的工作模式主要有八种：4种输入方式，4种输出方式。

分别为输入浮空，输入上拉，输入下拉，模拟输入；  
输出方式为开漏输出，开漏复用输出，推挽输出，推挽复用输出。

GPIO 8 种工作模式：

```
typedef enum{ GPIO_Mode_AIN = 0x0,  GPIO_Mode_IN_FLOATING = 0x04,  GPIO_Mode_IPD = 0x28,  GPIO_Mode_IPU = 0x48,  GPIO_Mode_Out_OD = 0x14,  GPIO_Mode_Out_PP = 0x10,  GPIO_Mode_AF_OD = 0x1C,  GPIO_Mode_AF_PP = 0x18 } GPIOMode_TypeDef;
```

## 4、为什么要先配置时钟，再配置GPIO？

所有寄存器都需要时钟才能配置，寄存器是由D触发器组成的，只有送来了时钟，触发器才能被改写值。

任何MCU的任何外设都需要有时钟，STM32为了让用户更好地掌握功耗，对每个外设的时钟都设置了开关，让用户可以精确地控制，关闭不需要的设备，达到节省供电的目的。

51单片机不用配置IO时钟，只是因为默认使用同一个时钟，这样是方便，但是这样的话功耗就降低不了。

例如，某个功能不需要，但是它还是一直运行。  
stm32需要配置时钟，就可以把不需要那些功能的功耗去掉。

当你想关闭某个IO的时候，关闭它相对应的时钟使能即可，不过在51里面，在使用IO的时候是没有设置IO的时钟的，在STM32中，有外部和内部时钟之分。

ARM的芯片都是这样，外设通常都是给了时钟后，才能设置它的寄存器（即才能使用这个外设）。STM32、LPC1XXX等等都是这样。  
这么做的目的是为了省电，使用了所谓时钟门控的技术。  
这也属于电路里同步电路的范畴：同步电路总是需要1个时钟。

## 二、以寄存器方式点亮流水灯

## 1、题目要求

假设你手中已有 STM32最小系统核心板(STM32F103C8T6)+面板板+3只红绿蓝LED，并搭建了电路，分别GPIOA-5、GPIOB-9、GPIOC-14 这3个引脚上控制LED灯（最高时钟2Mhz），轮流闪烁，间隔时长1秒。

1）写出程序设计思路，包括GPIOx端口的各寄存器地址和详细参数；

2）用C语言 寄存器方式编程实现。

## 2、建立工程文件

新建工程Light1文件，工程名为Light，选择STM32F103C8  
之后弹出的添加库文件窗口Manage Run-Time Environment，在这个界面，选择Cancel即可。

![](https://img-blog.csdnimg.cn/a4ce26afa0134356b6687cb32847fd03.png)

到这里，我们还只是建了一个框架，还需要添加启动代码，以及.c 文件等。

先介绍一下启动代码：启动代码是一段和硬件相关的汇编代码。  
主要作用如下：  
1、堆栈（SP）的初始化；  
2、初始化程序计数器（PC）；  
3、设置向量表异常事件的入口地址；  
4、调用 main 函数。

ST 公司提供了 3 个启动文件给我们，分别用于不同容量的 STM32 芯片，这三个文件是：  
startup\_stm32f10x\_ld.s,startup\_stm32f10x\_md.s,startup\_stm32f10x\_hd.s  
其中，ld.s 适用于小容量 产品；md.s 适用于中等容量产品；hd 适用于大容量产品；

其Flash容量为128K，属于中容量，因此我在这里采用`startup_stm32f10x_md.s`作为启动文件。

可以到这个网址去下载：http://www.openedv.com/posts/list/313.htm

![](https://img-blog.csdnimg.cn/f736f5a30f144bb3bfadb547b4fdff6d.png)  
将启动文件拷贝到Light工程文件夹下。

![](https://img-blog.csdnimg.cn/8f9a45e329ae42e28842e762874a5a83.png)

我们找到 Target1→Source Group1→双击→设置打开文件类型为 Asm Source file→选择 startup\_stm32f10x\_md.s→点击 Add，如下图所示:

![](https://img-blog.csdnimg.cn/d7b202a8a4dc48ae8222f374a52bd750.png)

 添加完成之后，得到以下画面：

![](https://img-blog.csdnimg.cn/5357acb33bfe44bdbc50eb1429988632.png)

先关闭 KEIL 软件,为了不让之后生成的文件显得混乱，在Light1文件夹下新建一个OBJ文件夹和USER文件夹， OBJ 用来存放这些编译过程中产生的中间文件(包括.hex 文件也将存放在这个文件夹里面)。USER 文件夹专门用来存放启动文件（startup\_stm32f10x\_md.s）、工程文件等不可缺少的文件，而然后把 DebugConfig和Listings 和Objects 文件夹全部移到 OBJ 文件夹下，剩余的全部移到USER文件夹里。  
由于上面我们还没有任何代码在工程里面，这里我们把系统代码 copy 过来（即 SYSTEM文件夹，该文件夹由 ALIENTEK 提供，这些代码在任何 STM32F10x 的芯片上都是通用的，可以用于快速构建自己的工程）。

![](https://img-blog.csdnimg.cn/228a687b59334b1eabadbba3fa171ca5.png)

在 USER 文件夹下面找到 light.`uvprojx`，打开它，然后在 Target 目录树上点击右键→`Manage Project Items`，弹出对话框。

![](https://img-blog.csdnimg.cn/b7d2987a059d4cfdb977920cf8bafe9c.png)

 在上面对话框的中间栏，点新建，新建 USER 和 SYSTEM 两个组。然后点击 Add Files 按钮，把 SYSTEM 文件夹三个子文件夹里面的：sys.c、usart.c、delay.c 加入到 SYSTEM 组中。注意：此时 USER 组下还是没有任何文件，我们只添加SYSTEM的三个。  
![](https://img-blog.csdnimg.cn/c5a55b68ea33404a826359a842361b25.png)

 上面都创建好之后，得到以下界面：

![](https://img-blog.csdnimg.cn/50aaad3da0c548c587e010c5a4fecad9.png)

 此时，USER文件夹里没有文件的，我们在USER文件夹下新建一个 test.c 文件，然后双击 USER 组，会弹出加载文件的对话框，此时我们在 USER 目录下选择test.c 文件，加入到 USER 组下。

![](https://img-blog.csdnimg.cn/561d71085e684223953ffd6a7150d874.png)

如果我们此时编译的话，生成的中间文件，还是会存放在 Listings 和 Objects 文件夹下，所以，我们先设置输出路径，再编译。

点击魔法棒，弹出 Options for Target’Target 1’对话框，选择 Output 选项卡→选中 Create Hex File（用于生成 Hex 文件，后面会用到）→点击 Select Folder for Objects→找到 OBJ 文件夹→点击 OK

 ![](https://img-blog.csdnimg.cn/729b36b950014df5864b2c8c23778e72.png)

 接着，再设置 Listings 文件路径，在图 3.2.16 的基础上，打开 Listing 选项卡→点击 Select  
Folder for Listings→找到 OBJ 文件夹→点击 OK

![](https://img-blog.csdnimg.cn/66d13d6737204f73b5496a362de72e7d.png)

 加入sys、delay、usart的include路径：

![](https://img-blog.csdnimg.cn/ef119fb161fe40d0830e20c0d7b3a4d6.png)

 注意！我们必须根据所用 STM32F1 型号的容量，来输入相关宏定义，对于STM32F103 系列芯片，设置原则如下：

> 16KB≤FLASH≤32KB 选择：STM32F10X\_LD 64KB≤FLASH≤128KB 选择：STM32F10X\_MD  
> 256KB≤FLASH≤512KB 选择：STM32F10X\_HD

 至此，一个完整的 STM32F1 开发工程模板下建立好了。接下来我们就可以进行代码下载和仿真调试了。

## 3、使用寄存器点亮LED灯——代码部分

### （1）硬件连接设计

根据题目要求，使用`GPIOB`,`GPIOC`,`GPIOD`端口来控制LED灯，在查询C8T6数据手册后，我选用了`PB5`,`PC4`,`PD8`管脚分别连接红绿蓝三种颜色的灯（由于我只有红绿黄三个小灯，在之后实际硬件中，会用黄灯代替蓝灯）

![](https://img-blog.csdnimg.cn/83c58521919c4f71a91f2c753da8b3f0.png)

图中从 3 个 LED 灯的阳极各经过 1 个限流电阻连接到 3.3V 电源，阴极连接STM32 的 3 个 GPIO 引脚中，所以我们只要控制这三个引脚输出高低电平，即可控制其所连接 LED 灯的亮灭。  
目标是把 GPIO 的引脚设置成推挽输出模式并且默认下拉，输出低电平，这样就能让 LED 灯亮起来了。

### （2）代开之前建立的工程，编写代码

在Light1文件夹下新建一个CODE文件夹，用来存储以后与硬件相关的代码。

![](https://img-blog.csdnimg.cn/b57f2571cb7c40a6bec031542a9871fc.png)

 然后我们打开 USER 文件夹下的Light.uvprojx 工程，新建两个文件，然后保存CODE→LED 文件夹下面，保存为 led.c，led.h  
我们将文件添加到工程中，步骤如下图：

![](https://img-blog.csdnimg.cn/35608cd1c39c40528d71c0a106809227.png)

 在魔法棒这里将CODE路径加进去，否则之后会报错。

![](https://img-blog.csdnimg.cn/addb2d66305649b997f57bd511cb1238.png)

 下面来编写led.c文件，要用到GPIOB、GPIOC、GPIOD则，对应时钟设置：

![](https://img-blog.csdnimg.cn/bd9f9f4e3f8d4e23ae7c783853dda033.png)

 在设置完时钟之后就是配置完时钟之后，LED\_Init 配置了 目标三个端口 PB0 PB5 PA1 的模式为推挽输出，  
并且默认输出 1。这样就完成了对这三个 IO 口的初始化。  
代码如下：

led.c

```
#include "led.h"void LED_Init(void){RCC->APB2ENR|=1<<2; RCC->APB2ENR|=1<<3; GPIOB->CRL&=0XFF0FFFFF; GPIOB->CRL|=0X00300000;GPIOB->ODR|=1<<5; GPIOB->CRL&=0XFFFFFFF0; GPIOB->CRL|=0X00000003;GPIOB->ODR|=1<<0; GPIOA->CRL&=0XFFFFFF0F; GPIOA->CRL|=0X00000030;GPIOA->ODR|=1<<1; }
```

led.h

```
#ifndef __LED_H#define __LED_H#include "sys.h"#define LED0 PBout(5) #define LED1 PBout(0) #define LED2 PAout(1) void LED_Init(void); #endif
```

 在USER文件夹的test.c中撰写main函数

test.c

```
#include "sys.h"#include "delay.h"#include "led.h"int main(void){ Stm32_Clock_Init(9); delay_init(72); LED_Init(); while(1){LED0=0;LED1=1;LED2=1;delay_ms(1000);LED0=1;LED1=0;LED2=1;delay_ms(1000);LED0=1;LED1=1;LED2=0;delay_ms(1000);} }
```

代码包含了#include "led.h"这句，使得 LED0、LED1、LED2、LED\_Init 等能在 main 函数里被调用。  
接下来，main 函数先调用 Stm32\_Clock\_Init 函数，配置系统时钟为 9 倍频，也就是 8\*9=72M（外部晶振是 8Mhz），然后调用 delay\_init 函数，初始化延时函数。接着就是调用 LED\_Init 来初始化 三个管脚 为输出。最后在死循环里面实现 LED0 LED1 LED2 交替闪烁，间隔为 1s。  
接下来编译代码，编译无误

![](https://img-blog.csdnimg.cn/e198edd231be4ee89e2b4223b30aab53.png)

 接下来就可以进来硬件操作了！

### （3）硬件连接

打开C8T6数据手册，查找TXD和RXD管脚位置

![](https://img-blog.csdnimg.cn/3a071906830142cab4cd5792a41918cd.png)

 务必将boot0设为1，boot1设为0，利用跳线帽实现

![](https://img-blog.csdnimg.cn/e04ceb55e2234991b4ec6d07e93d36fa.jpeg)

###  (4) 烧录：STM32F103C8T6与PC端连接

 借助FLYMCU下载软件，即可将Light.hex载入

![](https://img-blog.csdnimg.cn/2a8199546f8041c4b7645a48aaae2596.png)

##  四、使用寄存器点亮LED灯——电路部分

芯片面包板连好电路后，通电，LED满足实验要求

 在将C8T6连接到电路板之前，一定要先将BOOT0置位0，否则电路无效！！！

![](https://img-blog.csdnimg.cn/8a75191d222646bb859353f50b282406.jpeg)

 点灯实况

![](https://img-blog.csdnimg.cn/c7109e9788ae431dacb24732b571c948.gif)

##  五、stm32CubeMX+Keil使用[HAL库](https://so.csdn.net/so/search?q=HAL%E5%BA%93&spm=1001.2101.3001.7020)点亮流水灯

## 1、题目要求

（1）安装 stm32CubeMX，用cubemx完成初始化过程，采用HAL库编程实现。

（2）在Keil下用软件仿真运行上面代码，并用虚拟逻辑分析仪观察 对应管脚上的输出波形（高低电平转换），看是否是1秒的周期。

## 2、STM32CubeMX简介与下载

STM32CubeMX 是 ST 意法半导体近几年来大力推荐的STM32 芯片图形化配置工具，目的就是为了方便开发者， 允许用户使用图形化向导生成C 初始化代码，可以大大减轻开发工作，时间和费用，提高开发效率。STM32CubeMX几乎覆盖了STM32 全系列芯片。  
在CubeMX上，通过傻瓜化的操作便能实现相关配置，最终能够生成C语言代码，支持多种工具链，比如MDK、IAR For ARM、TrueStudio等 省去了我们配置各种外设的时间，大大的节省了时间。

关于STM32CubeMX的安装教程，可参考此博客[【STM32】STM32 CubeMx使用教程一--安装教程\_Z小旋的博客-CSDN博客\_cubemx](https://blog.csdn.net/as480133937/article/details/98885316 "【STM32】STM32 CubeMx使用教程一--安装教程_Z小旋的博客-CSDN博客_cubemx")

## 3、利用CubeMX新建工程点亮LED灯

### （1）前期准备

在本次实验中，使用到的STM32硬件为，STM32F103C8T6；  
软件为，STM32CubeMX软件、KEIL MDK-arm软件，以及STM32F1xxHAL库。

### （2）新建工程

**搜索芯片型号–>选择芯片–>创建工程**

1 在主界面选择`File`–>`New Project` 或者直接点击`ACCEE TO MCU SELECTOR`  
2 进行芯片型号选择，一般直接在左上角搜索自己的芯片型号即可。

![](https://img-blog.csdnimg.cn/2df896839650476a80f1251c538e6817.png)

 ![](https://img-blog.csdnimg.cn/69a6586b6e5f43a5a55fa4c37c5bcbfb.png)

###  （3）界面介绍

![](https://img-blog.csdnimg.cn/ab1f155b59dd447789e40d792083074f.png)

1.左侧为MCU外设资源选择  
（1）Categories 种类选择，将MCU各种外设和资源分类，供用户选择使用；  
（2）A-Z 顺序选择，MCU的外设资源按A-Z排序，供用户选择使用。  
2.中间为外设配置  
这里可以选择外设的各种功能，选择串口模式（异步、同步、半双工）串口接收终端，和串口DMA传输等。  
3.右侧为预览界面  
这里分为引脚预览和系统预览

引脚预览可以查看引脚配置了什么功能，和各个引脚的位置；  
任意点击一个引脚即可设置该引脚的各种功能。  
淡黄色这种颜色表示不可配置引脚，电源专用引脚以黄色突出显示，其配置不能更改。  
深黄色这种颜色表示你配置了一个I/O口的功能，但是没有初始化对应外设功能，引脚处于no mode状态。  
绿色表示配置成功。

系统预览是查看配置的各种外设和GPIO的状态  
![](https://img-blog.csdnimg.cn/4fefab37ac9d4ac4a40456fe5f8b2d58.png)

右侧第三个对勾图标，表示没有问题；  
右侧第二个图标则表示警告，对应配置出现问题，点击该选项即可在外设配置界面进行查看。

### （4）配置引脚

只需把目标LED对应引脚设置为GPIO\_Output即可

![](https://img-blog.csdnimg.cn/88e4201ce3d04bc6ae80f1c261dd02c6.png)

点击system core，进入里面的sys，在debug那里选择serial wire

![](https://img-blog.csdnimg.cn/682aae87d9e24f96a3820c487faee256.png)

 接下来进行配置时钟，进入RCC，有两个时钟HSE，LSE，我们要用GPIO借口，这些接口在APB2里  
观察时钟架构，APB2由HSE时钟控制，同时在这个界面把PLLCLK右边选上

![](https://img-blog.csdnimg.cn/12f1ea12551d4d659e9db41ce59ba0d9.png)

 所以我们把HSE那里设置为Crystal/Ceramic Resonator就行了

![](https://img-blog.csdnimg.cn/2b6e32722b4342f494151161722205e0.png)

### （5）配置工程环境

接下来就是点击相应的引脚设置输出寄存器，output哪一项，PA7、PA10、PB6

且在output level那一项选择high

![](https://img-blog.csdnimg.cn/28a0b4a22b9147f1a3ae3a22573b66f0.png)

 ![](https://img-blog.csdnimg.cn/b5ec30203ae1484b99273798260f2718.png)

点击Project Manager 配置好自己的路径名和项目名，然后改名

![](https://img-blog.csdnimg.cn/27f34f429495421d9b5352b343647eea.png)

 进入Code Generate界面，选择生成初始化文件.c/.h，之后在点击GENERATE CODE就行了

![](https://img-blog.csdnimg.cn/64afd85aa16a4581a3508df2c9f0001a.png)

 打开刚刚生成的项目将main.c的主函数，在while循环部分添加以下代码

```
HAL_GPIO_WritePin(GPIOA,GPIO_PIN_7,GPIO_PIN_RESET);HAL_GPIO_WritePin(GPIOA,GPIO_PIN_10,GPIO_PIN_SET);HAL_GPIO_WritePin(GPIOB,GPIO_PIN_6,GPIO_PIN_SET);HAL_Delay(1000);HAL_GPIO_WritePin(GPIOA,GPIO_PIN_7,GPIO_PIN_SET);HAL_GPIO_WritePin(GPIOA,GPIO_PIN_10,GPIO_PIN_RESET);HAL_GPIO_WritePin(GPIOB,GPIO_PIN_6,GPIO_PIN_SET);HAL_Delay(1000);HAL_GPIO_WritePin(GPIOA,GPIO_PIN_7,GPIO_PIN_SET);HAL_GPIO_WritePin(GPIOA,GPIO_PIN_10,GPIO_PIN_SET);HAL_GPIO_WritePin(GPIOB,GPIO_PIN_6,GPIO_PIN_RESET);HAL_Delay(1000);
```

![](https://img-blog.csdnimg.cn/c7ffd55cae3e49049256ff18541cb6f8.png)

![](https://img-blog.csdnimg.cn/203cb976f53a447da6a114ddd613214a.png)

 提醒：所有自己编写的代码请放在/\* USER CODE BEGIN XXX _/ /_ USER CODE END XXX \*/之间；  
这样我们修改工程的时候你自己写的代码就不会被删除。

## 4、编译及烧录

编译发现没有报错，那么进行烧录啦

![](https://img-blog.csdnimg.cn/026397456a204827a278e135f0d115d0.png)

 将C8T6与USB转TTL相连，接入电脑端，打开FlyMcu进行烧录

![](https://img-blog.csdnimg.cn/6dae0e4b91664576a3658220b1890755.png)

效果图如下：![](https://img-blog.csdnimg.cn/b918391dc9ee40cda0ac44198e9f390c.jpeg)

##  五、总结

通过此次实验，可以深刻了解STM32F103寄存器方式点亮LED流水灯的方式，同时对寄存器和keil操作有了更加深刻的认识，HAL库提供的可视化界面大大提高了编程效率，无需再利用代码去配置管脚了，效果很好，受益匪浅，通过烧录程序，更加增强了动手操作能力；而且，面包板是个很方便的东西，比焊接快多了，以后可以多多利用。

## 六、参考文献

[stm32 GPIO简单介绍及初始化配置（库函数）\_朕不是皇的博客-CSDN博客\_gpio初始化](https://blog.csdn.net/asdfg1075511750/article/details/79663568 "stm32 GPIO简单介绍及初始化配置（库函数）_朕不是皇的博客-CSDN博客_gpio初始化")

 [【STM32】STM32 CubeMx使用教程一--安装教程\_Z小旋的博客-CSDN博客\_cubemx](https://blog.csdn.net/as480133937/article/details/98885316 "【STM32】STM32 CubeMx使用教程一--安装教程_Z小旋的博客-CSDN博客_cubemx")

https://blog.csdn.net/qq\_46467126/article/details/120737655  
https://blog.csdn.net/qq\_46467126/article/details/120791793  
https://blog.csdn.net/geek\_monkey/article/details/86293880