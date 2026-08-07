---
title: "RT-Thread RECA学习营（一）——正点原子战舰 V3  RT-Thread BSP 移植"
source: https://asurada.zone/post/RT-Thread_BSP/
date: 2020-05-06
updated: 2020-05-06
tags: [RT-Thread, STM32]
categories: [嵌入式]
---

# RT-Thread RECA学习营（一）——正点原子战舰 V3  RT-Thread BSP 移植

## 前言

最近在准备六月份的 RT-Thread 的 [RECA](https://www.rt-thread.org/page/rac.html)（RT-Thread开发者能力认证），官方组织了一个学习营，第一周的任务是：

> 基于 rt-thread4.0.2，能根据BSP制作教程及群里分享的视频《2.RTThread移植》，让自己的板子在RT-Thread上运行起来，了解BSP的制作。

本文并没有将 BSP 移植背后的原理全部弄清楚，只是想着先迈出第一步，不求甚解，先把整个流程给跑通。

我手上的开发板是正点原子的战舰 V3，所以我将基于 RT-Thread 官方的 STM32F1 系列 BSP 通用模板构建战舰 V3 的 BSP。

## 下载 RT-Thread 源码

从[官网](https://www.rt-thread.org/page/download.html)下载 RT-Thread 源码，我下载的版本是 V4.0.2。源码目录结构如下：

![RTThread文件目录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/1-RTThread%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95.png)

| 目录名 | 描述 |
| --- | --- |
| rt-thread | RT-Thread 的源文件。 |
| \- bsp | RT-Thread 官方已经支持的 BSP。 |
| \- components | RT-Thread 的各个组件目录。 |
| \- documentation | RT-Thread 相关文档。 |
| \- examples | RT-Thread 相关例程。 |
| \- include | RT-Thread 内核的头文件。 |
| \- libcpu | 各类芯片的移植代码，此处包含了 STM32 的移植文件。 |
| \- src | RT-Thread 内核的源文件。 |
| \- tools | RT-Thread 命令构建工具的脚本文件。 |

## 复制通用模板

制作新 BSP 的第一步是复制一份同系列的 BSP 模板作为基础，通过对 BSP 模板的修改来获得新 BSP。战舰 V3 采用的芯片是 STM32F103ZET6，所以我们需要使用 STM32F1 系列的模板。模板位于`rt-thread\bsp\stm32\libraries\templates\stm32f10x`，BSP 模板文件夹结构如下所示：

![BSP模板目录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/5-BSP%E6%A8%A1%E6%9D%BF%E7%9B%AE%E5%BD%95.png)

拷贝模板文件夹下的 `stm32f10x` 文件夹到`rt-thread\bsp\stm32`下，并将该文件夹的名称改为 `stm32f1-my-diy`，之后我们就在这个文件夹的基础上进行移植。

在接下来的 BSP 的制作过程中，将会修改 board 文件夹内的配置文件，将 F1 系列的 BSP 模板变成一个适用于正点原子战舰 V3 开发板的 BSP ，下表总结了 board 文件夹中需要修改的内容：

| 项目 | 需要修改的内容说明 |
| --- | --- |
| CubeMX\_Config （文件夹） | CubeMX 工程 |
| linker\_scripts （文件夹） | BSP 特定的链接脚本 |
| board.c/h | 系统时钟、GPIO 初始化函数、芯片存储器大小 |
| Kconfig | 芯片型号、系列、外设资源 |
| SConscript | 芯片启动文件、目标芯片型号 |

## 安装 CubeMX

### 下载 CubeMX

CubeMX 是基于 eclipse 的一个插件，用来对STM32产品的配置及初始化代码的生成。下载可以去 [ST 中国站](https://stmcu.com.cn/) –> 设计资源 –> 固件和软件 –> PC 端软件 –> [STM32CubeMX](https://stmcu.com.cn/Designresource/design_resource_detail?file_name=STM32CubeMX_STM32%E5%88%9D%E5%A7%8B%E5%8C%96%E4%BB%A3%E7%A0%81%E7%94%9F%E6%88%90%E5%99%A8&lang=EN&ver=5.6.1)

我下载的版本是 2020.04.21 更新的 V5.6.1。

在官网上下载要求先登陆账号，所以没有账号的同学需要先自行注册一个账号，中国站可以直接通过微信注册。

![CubeMX下载路径图](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/10-CubeMX下载路径图.png)

### 安装 CubeMX

前面已经说了 CubeMX 是基于 eclipse 的，所以它的运行需要依赖 Java 运行环境，后续具体的步骤可以参考[STM32CubeMX系列教程02\_STM32CubeMX工具、HAL库下载、安装说明](https://www.strongerhuang.com/STM32Cube/STM32CubeMX/STM32CubeMX%E7%B3%BB%E5%88%97%E6%95%99%E7%A8%8B02_STM32CubeMX%E5%B7%A5%E5%85%B7%E3%80%81HAL%E5%BA%93%E4%B8%8B%E8%BD%BD%E3%80%81%E5%AE%89%E8%A3%85%E8%AF%B4%E6%98%8E.html)，写得非常详细。

## 使用 CubeMX 配置工程

模板文件内已经有 CubeMX 工程了，位于`stm32f1-my-diy\board\CubeMX_Config`下，双击打开 `CubeMX_Config.ioc` 工程。

![双击打开CubeMX工程](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/6-%E5%8F%8C%E5%87%BB%E6%89%93%E5%BC%80CubeMX%E5%B7%A5%E7%A8%8B.png)

### 配置芯片

我使用的是正点原子战舰 V3 的开发板，使用的 CPU 是 STM32F103ZET6，CubeMX 模板工程使用的并不是这个芯片，所以我需要重新配置芯片为「STM32F103ZETx」。

![CubeMX选择芯片](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/11-CubeMX%E9%80%89%E6%8B%A9%E8%8A%AF%E7%89%87.png)

### 启用系统时钟

紧接着配置系统时钟，启用高速外部时钟(HSE)和低速外部时钟(LSE)：

![CubeMX启用系统时钟](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/12-CubeMX%E5%90%AF%E7%94%A8%E7%B3%BB%E7%BB%9F%E6%97%B6%E9%92%9F.png)

### 打开串口外设

Connectivity –> USART1 –> Mode:Asynchronous，将 USART1 配置为异步模式。

注意这里只需要选择串口外设引脚即可，无需配置其他参数。这里配置串口是为了后续使用 RT-Thread 的 FinSH 组件。

![CubeMX配置USART1](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/14-CubeMXUSART1%E5%BC%95%E8%84%9A%E5%9B%BE.png)

配置完成后，我们可以把右边的芯片图放大，然后就能看到芯片右侧的 PA10、PA9 引脚变成了绿色，后面还跟了当前引脚的功能，其中 PA10 引脚是 UASRT1 的接收引脚，PA9 是 UASRT1 的发送引脚。

![CubeMXUSART1引脚图](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/13-CubeMX%E9%85%8D%E7%BD%AEUSART1.png)

### 设置调试方式

System Core –> SYS –> Debug:Serial Wire，将调试配置 SWD 模式，这种模式一个最大的优点就是占用的 IO 口较少，我们从右侧的芯片图可以看到 SWD 模式只使用了两个 IO 口 PA13 和 PA14。

![CubeMX配置调试模式](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/15-CubeMX%E9%85%8D%E7%BD%AE%E8%B0%83%E8%AF%95%E6%A8%A1%E5%BC%8F.png)

### 时钟配置

HSE 是高速外部时钟，可接石英/陶瓷谐振器，或者接外部时钟源，频率范围为 4MHz~16MHz。战舰 V3 开发板接的是 8M 的晶振。

LSE 是低速外部时钟，接频率为 32.768kHz 的石英晶体。 这个主要是 RTC 的时钟源。

根据正点原子《STMF1 开发指南》，这一步进行的时钟配置为：

-   启用 HSE 作为 PLL 时钟源
-   设置 PLL 9 倍倍频
-   选择系统时钟源为 PLL
-   设置 APB1 分频系数为 2

![CubeMX配置时钟](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/16-CubeMX%E9%85%8D%E7%BD%AE%E6%97%B6%E9%92%9F.png)

最后总结一下配置过后关键时钟频率值：

```abnf
SYSCLK(系统时钟) 			   =72MHz
PLL 主时钟 					=72MHz
AHB 总线时钟（HCLK=SYSCLK/1）   =72MHz
APB1 总线时钟（PCLK1=HCLK/2）   =36MHz
APB2 总线时钟（PCLK2=HCLK/1）   =72MHz
```

### 生成 CubeMX 项目

![CubeMX生成代码](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/17-CubeMX%E7%94%9F%E6%88%90%E4%BB%A3%E7%A0%81.png)

最后生成的时候，因为我们复制过来的模板工程里面本来就有 CubeMX 项目，选择直接覆盖即可。

### 拷贝时钟初始化函数

在 `board/board.c` 文件中存放了函数 `SystemClock_Config()` ，该函数负责初始化系统时钟。当使用 CubeMX 工具对系统时钟重新配置的时候，需要更新这个函数。

该函数由 CubeMX 工具生成，默认存放在`board/CubeMX_Config/Src/main.c` 文件中。但是该文件并没有被包含到我们的工程中，因此需要将这个函数从 main.c 中拷贝到 `board/board.c` 文件中。该函数内容如下：

```c
/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;	//时钟源为 HSE
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;						//打开 HSE
  RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;		//HSE 预分频
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;						//打开 HSI
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;					//打开 PLL
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;			//PLL 时钟源选择 HSE
  RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;					//设置 PLL 倍频系数
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;		//设置系统时钟时钟源为 PLL
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;	//AHB 分频系数为1
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;		//APB1 分频系数为2
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;		//APB2 分频系数为1

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}
```

### 修改 board.h

在 board.h 文件中配置了 FLASH 和 RAM 的相关参数，这个文件中需要修改的是 STM32\_FLASH\_SIZE 和 STM32\_SRAM\_SIZE 这两个宏控制的参数。

战舰 V3 所使用的 STM32F103ZET6 芯片的 FLASH 大小为 512K，RAM 的大小为 64K，因此对该文件作出如下的修改：

![修改board头文件](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/18-%E4%BF%AE%E6%94%B9board%E5%A4%B4%E6%96%87%E4%BB%B6.png)

## 修改 Kconfig 选项

在本小节中修改 `board/Kconfig` 文件的内容有如下两点：

-   芯片型号和系列
-   BSP 上的外设支持选项

芯片型号和系列的修改如下表所示：

| 宏定义 | 意义 | 格式 |
| --- | --- | --- |
| SOC\_STM32F103ZE | 芯片型号 | SOC\_STM32xxx |
| SOC\_SERIES\_STM32F1 | 芯片系列 | SOC\_SERIES\_STM32xx |

关于 BSP 上的外设支持选项，一个初次提交的 BSP 仅仅需要支持 GPIO 驱动和串口驱动即可，因此在配置选项中只需保留这两个驱动配置项，如下图所示：

![Kconfig选项](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/2-Kconfig%E9%80%89%E9%A1%B9.png)

## 修改工程构建相关文件

接下来需要修改用于构建工程相关的文件。

### 修改链接脚本

linker\_scripts 链接文件如下图所示：

![链接脚本目录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/19-%E9%93%BE%E6%8E%A5%E8%84%9A%E6%9C%AC%E7%9B%AE%E5%BD%95.png)

其中 MDK 使用的链接脚本是 link.sct ，其他两个链接脚本的文件分别为 IAR 使用的 link.icf 和 GCC 编译器使用的 link.lds。我使用的是 MDK，所以修改 link.sct 文件。

战舰 V3 使用的芯片为 STM32F103ZE，FLASH 为 512K，因此修改 LR\_IROM1 和 ER\_IROM1 的参数为 0x00080000（512 \* 1024）。RAM 的大小为 64K， 因此修改 RW\_IRAM1 的参数为 0x00010000（64 \* 1024）。

![修改链接脚本](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/20-%E4%BF%AE%E6%94%B9%E9%93%BE%E6%8E%A5%E8%84%9A%E6%9C%AC.png)

### 修改 SConscript 构建脚本

SConscript 脚本决定 MDK/IAR 工程的生成以及编译过程中要添加文件。

在这一步中需要修改芯片型号以及芯片启动文件的地址，战舰 V3 使用的芯片是 STM32F103ZET6，所以使用的启动文件是 `startup_stm32f103xe.s`，对应的芯片型号是 STM32F103xE。最后修改内容如下图所示：

![修改SConscript构建脚本](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/21-%E4%BF%AE%E6%94%B9SConscript%E6%9E%84%E5%BB%BA%E8%84%9A%E6%9C%AC.png)

### 修改 MDK 工程模板

template 文件是生成 MDK/IAR 工程的模板文件，通过修改该文件可以设置工程中使用的芯片型号以及下载方式。MDK4/MDK5/IAR 的工程模板文件，如下图所示：

![工程模板目录](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/22-%E5%B7%A5%E7%A8%8B%E6%A8%A1%E6%9D%BF%E7%9B%AE%E5%BD%95.png)

我使用的 MDK5，所以我修改的模板是 `template.uvprojx` 文件。

-   战舰 V3 所使用的的芯片型号为 `STM32F103ZET6`，所以修改芯片型号如下：

![MDK5模板工程修改芯片型号](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/23-MDK5%E6%A8%A1%E6%9D%BF%E5%B7%A5%E7%A8%8B%E4%BF%AE%E6%94%B9%E8%8A%AF%E7%89%87%E5%9E%8B%E5%8F%B7.png)

-   修改程序下载方式：

![MDK5模板工程修改默认下载方式](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/24-MDK5%E6%A8%A1%E6%9D%BF%E5%B7%A5%E7%A8%8B%E4%BF%AE%E6%94%B9%E9%BB%98%E8%AE%A4%E4%B8%8B%E8%BD%BD%E6%96%B9%E5%BC%8F.png)

修改完成后**记得 `Ctrl+S`保存**。

## 安装 ENV 工具

### Env 工具简介

Env 是 RT-Thread 推出的开发辅助工具，针对基于 RT-Thread 操作系统的项目工程，提供编译构建环境、图形化系统配置及软件包管理功能。

其内置的 menuconfig 提供了简单易用的配置剪裁工具，可对内核、组件和软件包进行自由裁剪，使系统以搭积木的方式进行构建。

Env 工具包含了 RT-Thread 源代码开发编译环境和软件包管理系统。

### 下载安装 Env 工具

-   从 RT-Thread 官网下载 [Env 工具](https://www.rt-thread.org/page/download.html)，将其**解压到任一纯英文路径下**即可使用，在 Env 目录下有一张 `Add_Env_To_Right-click_Menu.png`(添加 Env 至右键菜单.png) 的图片，按照图片指引一步步操作，就可以在任意文件夹下通过右键菜单来启动 Env 控制台，效果如下：![任意路径右键打开Env](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/26-menuconfig%E7%95%8C%E9%9D%A2.png)
    
-   在电脑上装好 git，软件包管理功能需要 git 的支持。git 的下载地址为[https://git-scm.com/downloads，根据向导正确安装](https://git-scm.com/downloads%EF%BC%8C%E6%A0%B9%E6%8D%AE%E5%90%91%E5%AF%BC%E6%AD%A3%E7%A1%AE%E5%AE%89%E8%A3%85) git，并将 git 添加到系统环境变量。
    
-   注意在工作环境中，所有的路径都不可以有中文字符或者空格。
    

## 重新生成 MDK 工程

### 重新生成 rtconfig.h 文件

在 env 界面输入命令 `menuconfig` 对工程进行配置。

![menuconfig界面](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/25-%E4%BB%BB%E6%84%8F%E8%B7%AF%E5%BE%84%E5%8F%B3%E9%94%AE%E6%89%93%E5%BC%80Env.png)

#### menuconfig 快捷键介绍

![menuconfig快捷键介绍](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/27-menuconfig%E5%BF%AB%E6%8D%B7%E9%94%AE%E4%BB%8B%E7%BB%8D.png)

#### 修改配置

menuconfig 有多种类型的配置项，修改方法也有所不同，常见类型如下：

-   开/关 型：使用空格键来选中或者关闭
-   数值、字符串型：按下回车键后会出现对话框，在对话框中对配置项进行修改

我们目前的目的仅仅是移植 BSP，所以先不进行复杂的配置，只使能 GPIO 和 UART：

-   Hardware Drivers Config –> On-chip Peripheral Drivers –> 使能「Enable GPIO」和「Enable UART」
-   「Enable UART」 –> 「Enable UART1 RX DMA」

#### 保存配置

选择好配置项之后按 ESC 键退出，选择保存修改即可自动更新 rtconfig.h 文件。此时再次使用 scons 命令就会根据新的 rtconfig.h 文件重新编译工程了。

### 重新生成 MDK5 工程

使用 env 工具输入命令 `scons --target=mdk5` 重新生成工程，如下图所示：

![重新生成MDK5工程](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/28-%E9%87%8D%E6%96%B0%E7%94%9F%E6%88%90MDK5%E5%B7%A5%E7%A8%8B.png)

重新生成工程成功：

![重新生成MDK5工程成功](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/29-%E9%87%8D%E6%96%B0%E7%94%9F%E6%88%90MDK5%E5%B7%A5%E7%A8%8B%E6%88%90%E5%8A%9F.png)

生成新的 MDK5 工程以后，我们可以看到 `project.uvprojx` 的修改日期已经变成刚刚进行生成操作时的日期了。

### 验证 MDK5 工程

双击打开 `project.uvprojx`，检查后发现在我们没有进行设置的情况下，此工程芯片型号、默认下载设置都和我们之前对模板工程的修改保持一致，说明我们之前的修改是生效了的。

我们通过左边的项目框架里可以看到，相比模板工程的空空如也，新工程已经是五脏俱全了。打开 `Applications --> main.c`，内容很简单，在 main 主函数内只有一个让 LED0 闪烁的程序，直接编译，没有任何错误和警告。为了方便调试和及时查看运行结果，设置程序下载结束后自动重启并运行：

![MDK5修改下载后重启](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/30-MDK5%E4%BF%AE%E6%94%B9%E4%B8%8B%E8%BD%BD%E5%90%8E%E9%87%8D%E5%90%AF.png)

连上战舰 V3 开发板，直接烧录程序，会发现 LED 并没有亮起来，检查代码后会发现，程序默认定义的 LED0 是引脚 PB.1，通过而查看战舰开发板的原理图，发现战舰开发板的 LED0 连接的是 PB.5 引脚，将引脚进行更改后编译，再烧入程序，LED0 正常闪烁。

![验证MDK5工程](https://blogimage-1256221427.cos.ap-guangzhou.myqcloud.com/31-%E9%AA%8C%E8%AF%81MDK5%E5%B7%A5%E7%A8%8B.png)

## 后记

至此，我们的 BSP 移植就算成功了。但回顾整个移植过程，发现自己不明白的地方还太多了，特别是如何由模板工程直接构建出一个能够直接运行的工程，在没有了解其中原理之前，这个过程对我这个菜鸟而言无异于「魔法」；我开始移植的基础是 RT-Thread 内置的 STM32F1 系列通用模板，这个模板我是拿过来就用的，但这个模板是如何一步步构建起来的，我也完全没有弄清楚；CubeMX 图形化的操作方式固然对新手非常友好，但无疑也让我对底层代码的理解隔了一层「黑盒」。

第一步已经迈出去了，希望通过后面的学习能够解答我上面诸多疑惑，学习不就正是这样一个发现问题，解决问题的过程。

## 参考链接

[RT-Thread 官方 STM32 系列 BSP 制作教程](https://github.com/RT-Thread/rt-thread/blob/master/bsp/stm32/docs/STM32%E7%B3%BB%E5%88%97BSP%E5%88%B6%E4%BD%9C%E6%95%99%E7%A8%8B.md)
