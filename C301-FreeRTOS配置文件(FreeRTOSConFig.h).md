---
html:
    toc: true
---

# <font face="仿宋" font color=#409EFF><center>文件目录</center></font>
## <center><font face ="楷体" sice=5>鑫</font></center>
### 0.前言
FreeRTOS内核是高度可定制的，使用配置文件FreeRTOSConfig.h进行定制。

每个FreeRTOS应用都必须包含这个头文件，用户根据实际应用来裁剪定制FreeRTOS内核。

这个配置文件是针对用户程序的，而非内核，因此配置文件一般放在应用程序目录下，不要放在RTOS内核源码目录下。

在下载的FreeRTOS文件包中，每个演示例程都有一个FreeRTOSConfig.h文件。有些例程的配置文件是比较旧的版本，可能不会包含所有有效选项。如果没有在配置文件中指定某个选项，那么RTOS内核会使用默认值。典型的FreeRTOSConfig.h配置文件定义如下所示

```
#ifndef FREERTOS_CONFIG_H
#define FREERTOS_CONFIG_H
 
#include "sys.h"
#include "usart.h"
//针对不同的编译器调用不同的stdint.h文件
#if defined(__ICCARM__) || defined(__CC_ARM) || defined(__GNUC__)
    #include <stdint.h>
    extern uint32_t SystemCoreClock;
#endif
 
//断言
#define vAssertCalled(char,int) printf("Error:%s,%d
",char,int)
#define configASSERT(x) if((x)==0) vAssertCalled(__FILE__,__LINE__)
 
/***************************************************************************************/
/*                                        FreeRTOS基础配置配置选项                                              */
/***************************************************************************************/
#define configUSE_PREEMPTION                    1   //1使用抢占式内核，0使用协程
#define configUSE_TIME_SLICING                  1   //1使能时间片调度(默认式使能的)
#define configUSE_PORT_OPTIMISED_TASK_SELECTION  1  //1启用特殊方法来选择下一个要运行的任务
                                                   //一般是硬件计算前导零指令，如果所使用的
                                                 //MCU没有这些硬件指令的话此宏应该设置为0！
#define configUSE_TICKLESS_IDLE                 0    //1启用低功耗tickless模式
#define configUSE_QUEUE_SETS                    1     //为1时启用队列
#define configCPU_CLOCK_HZ         (SystemCoreClock)   //CPU频率
#define configTICK_RATE_HZ                   (1000)    //时钟节拍频率，这里设置为1000，周期就是1ms
#define configMAX_PRIORITIES             (32)              //可使用的最大优先级
#define configMINIMAL_STACK_SIZE   ((unsigned short)130)   //空闲任务使用的堆栈大小
#define configMAX_TASK_NAME_LEN           (16)             //任务名字字符串长度
 
#define configUSE_16_BIT_TICKS          0    //系统节拍计数器变量数据类型，
                                             //1表示为16位无符号整形，0表示为32位无符号整形
#define configIDLE_SHOULD_YIELD         1    //为1时空闲任务放弃CPU使用权给其他同优先级的用户任务
#define configUSE_TASK_NOTIFICATIONS    1    //为1时开启任务通知功能，默认开启
#define configUSE_MUTEXES               1    //为1时使用互斥信号量
#define configQUEUE_REGISTRY_SIZE       8    //不为0时表示启用队列记录，具体的值是可以
                                             //记录的队列和信号量最大数目。
#define configCHECK_FOR_STACK_OVERFLOW   0   //大于0时启用堆栈溢出检测功能，如果使用此功能
                                             //用户必须提供一个栈溢出钩子函数，如果使用的话
                                             //此值可以为1或者2，因为有两种栈溢出检测方法。
#define configUSE_RECURSIVE_MUTEXES      1   //为1时使用递归互斥信号量
#define configUSE_MALLOC_FAILED_HOOK     0   //1使用内存申请失败钩子函数
#define configUSE_APPLICATION_TASK_TAG   0                       
#define configUSE_COUNTING_SEMAPHORES    1   //为1时使用计数信号量
 
/***************************************************************************************/
/*                                FreeRTOS与内存申请有关配置选项                                                */
/***************************************************************************************/
#define configSUPPORT_DYNAMIC_ALLOCATION     1         //支持动态内存申请
#define configTOTAL_HEAP_SIZE        ((size_t)(20*1024))     //系统所有总的堆大小
 
/***************************************************************************************/
/*                                FreeRTOS与钩子函数有关的配置选项                                              */
/***************************************************************************************/
#define configUSE_IDLE_HOOK           0          //1，使用空闲钩子；0，不使用
#define configUSE_TICK_HOOK           0         //1，使用时间片钩子；0，不使用
 
/***************************************************************************************/
/*                                FreeRTOS与运行时间和任务状态收集有关的配置选项                                 */
/***************************************************************************************/
#define configGENERATE_RUN_TIME_STATS           0         //为1时启用运行时间统计功能
#define configUSE_TRACE_FACILITY                1         //为1启用可视化跟踪调试
#define configUSE_STATS_FORMATTING_FUNCTIONS    1         //与宏configUSE_TRACE_FACILITY同时为1时会编译下面3个函数
                                                                        //prvWriteNameToBuffer(),vTaskList(),
                                                                        //vTaskGetRunTimeStats()
                                                                        
/***************************************************************************************/
/*                                FreeRTOS与协程有关的配置选项                                                  */
/***************************************************************************************/
#define configUSE_CO_ROUTINES             0        //为1时启用协程，启用协程以后必须添加文件croutine.c
#define configMAX_CO_ROUTINE_PRIORITIES     ( 2 )    //协程的有效优先级数目
 
/***************************************************************************************/
/*                                FreeRTOS与软件定时器有关的配置选项                                            */
/***************************************************************************************/
#define configUSE_TIMERS            1      //为1时启用软件定时器
#define configTIMER_TASK_PRIORITY  (configMAX_PRIORITIES-1)        //软件定时器优先级
#define configTIMER_QUEUE_LENGTH    5      //软件定时器队列长度
#define configTIMER_TASK_STACK_DEPTH (configMINIMAL_STACK_SIZE*2) //软件定时器任务堆栈大小
 
/***************************************************************************************/
/*                                FreeRTOS可选函数配置选项                                                      */
/***************************************************************************************/
#define INCLUDE_xTaskGetSchedulerState          1                       
#define INCLUDE_vTaskPrioritySet                1
#define INCLUDE_uxTaskPriorityGet               1
#define INCLUDE_vTaskDelete                     1
#define INCLUDE_vTaskCleanUpResources           1
#define INCLUDE_vTaskSuspend                    1
#define INCLUDE_vTaskDelayUntil                 1
#define INCLUDE_vTaskDelay                      1
#define INCLUDE_eTaskGetState                   1
#define INCLUDE_xTimerPendFunctionCall          1
 
/***************************************************************************************/
/*                                FreeRTOS与中断有关的配置选项                                                  */
/***************************************************************************************/
#ifdef __NVIC_PRIO_BITS
    #define configPRIO_BITS               __NVIC_PRIO_BITS
#else
    #define configPRIO_BITS               4                  
#endif
 
#define configLIBRARY_LOWEST_INTERRUPT_PRIORITY        15     //中断最低优先级
#define configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY   5     //系统可管理的最高中断优先级
#define configKERNEL_INTERRUPT_PRIORITY         ( configLIBRARY_LOWEST_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )
#define configMAX_SYSCALL_INTERRUPT_PRIORITY     ( configLIBRARY_MAX_SYSCALL_INTERRUPT_PRIORITY << (8 - configPRIO_BITS) )
 
/***************************************************************************************/
/*                                FreeRTOS与中断服务函数有关的配置选项                                          */
/***************************************************************************************/
#define xPortPendSVHandler     PendSV_Handler
#define vPortSVCHandler     SVC_Handler
 
#endif /* FREERTOS_CONFIG_H */
```
### 1.configUSE_PREEMPTION
为1时RTOS使用抢占式调度器，为0时RTOS使用协作式调度器（时间片）。
注：在多任务管理机制上，操作系统可以分为抢占式和协作式两种。协作式操作系统是任务主动释放CPU后，切换到下一个任务。任务切换的时机完全取决于正在运行的任务。

### 2.configUSE_PORT_OPTIMISED_TASK_SELECTION
某些运行FreeRTOS的硬件有两种方法选择下一个要执行的任务：通用方法和特定于硬件的方法（以下简称“特殊方法”）。
**通用方法：**
configUSE_PORT_OPTIMISED_TASK_SELECTION设置为0或者硬件不支持这种特殊方法。

可以用于所有FreeRTOS支持的硬件。

完全用C实现，效率略低于特殊方法。

不强制要求限制最大可用优先级数目

**特殊方法：**

并非所有硬件都支持。

必须将configUSE_PORT_OPTIMISED_TASK_SELECTION设置为1。

依赖一个或多个特定架构的汇编指令（一般是类似计算前导零[CLZ]指令）。

比通用方法更高效。

一般强制限定最大可用优先级数目为32。

### 3.configUSE_TICKLESS_IDLE
设置configUSE_TICKLESS_IDLE为1使能低功耗tickless模式，为0保持系统节拍（tick）中断一直运行。
通常情况下，FreeRTOS回调空闲任务钩子函数（需要设计者自己实现），在空闲任务钩子函数中设置微处理器进入低功耗模式来达到省电的目的。因为系统要响应系统节拍中断事件，因此使用这种方法会周期性的退出、再进入低功耗状态。如果系统节拍中断频率过快，则大部分电能和CPU时间会消耗在进入和退出低功耗状态上。
FreeRTOS的tickless空闲模式会在空闲周期时停止周期性系统节拍中断。停止周期性系统节拍中断可以使微控制器长时间处于低功耗模式。移植层需要配置外部唤醒中断，当唤醒事件到来时，将微控制器从低功耗模式唤醒。微控制器唤醒后，会重新使能系统节拍中断。由于微控制器在进入低功耗后，系统节拍计数器是停止的，但我们又需要知道这段时间能折算成多少次系统节拍中断周期，这就需要有一个不受低功耗影响的外部时钟源，即微处理器处于低功耗模式时它也在计时的，这样在重启系统节拍中断时就可以根据这个外部计时器计算出一个调整值并写入RTOS 系统节拍计数器变量中。

### 4.configUSE_IDLE_HOOK
设置为1使用空闲钩子（Idle Hook类似于回调函数），0忽略空闲钩子。

当RTOS调度器开始工作后，为了保证至少有一个任务在运行，空闲任务被自动创建，占用最低优先级（0优先级）。对于已经删除的RTOS任务，空闲任务可以释放分配给它们的堆栈内存。因此，在应用中应该注意，使用vTaskDelete()函数时要确保空闲任务获得一定的处理器时间。除此之外，空闲任务没有其它特殊功能，因此可以任意的剥夺空闲任务的处理器时间。

应用程序也可能和空闲任务共享同个优先级。

空闲任务钩子是一个函数，这个函数由用户来实现，RTOS规定了函数的名字和参数，这个函数在每个空闲任务周期都会被调用。

要创建一个空闲钩子：

设置FreeRTOSConfig.h 文件中的configUSE_IDLE_HOOK 为1；

定义一个函数，函数名和参数如下所示：

`void vApplicationIdleHook(void );  `
这个钩子函数不可以调用会引起空闲任务阻塞的API函数（例如：vTaskDelay()、带有阻塞时间的队列和信号量函数），在钩子函数内部使用协程是被允许的。

使用空闲钩子函数设置CPU进入省电模式是很常见的。

### 5.configUSE_MALLOC_FAILED_HOOK
每当一个任务、队列、信号量被创建时，内核使用一个名为pvPortMalloc()的函数来从堆中分配内存。官方的下载包中包含5个简单内存分配策略，分别保存在源文件heap_1.c、heap_2.c、heap_3.c、heap_4.c、heap_5.c中。仅当使用这五个简单策略之一时，宏configUSE_MALLOC_FAILED_HOOK才有意义。

如果定义并正确配置malloc()失败钩子函数，则这个函数会在pvPortMalloc()函数返回NULL时被调用。只有FreeRTOS在响应内存分配请求时发现堆内存不足才会返回NULL。

如果宏configUSE_MALLOC_FAILED_HOOK设置为1，那么必须定义一个malloc()失败钩子函数，如果宏configUSE_MALLOC_FAILED_HOOK设置为0，malloc()失败钩子函数不会被调用，即便已经定义了这个函数。malloc()失败钩子函数的函数名和原型必须如下所示：

### 6.configUSE_TICK_HOOK
设置为1使用时间片钩子（Tick Hook），0忽略时间片钩子。

<font face="仿宋" font color=#FF0000>注：时间片钩子函数（Tick Hook Function）</font>

时间片中断可以周期性的调用一个被称为钩子函数（回调函数）的应用程序。时间片钩子函数可以很方便的实现一个定时器功能。

只有在FreeRTOSConfig.h中的configUSE_TICK_HOOK设置成1时才可以使用时间片钩子。一旦此值设置成1，就要定义钩子函数，函数名和参数如下所示：

`void vApplicationTickHook( void ); `
vApplicationTickHook()函数在中断服务程序中执行，因此这个函数必须非常短小，不能大量使用堆栈，不能调用以”FromISR" 或 "FROM_ISR”结尾的API函数。

在FreeRTOSVx.x.xFreeRTOSDemoCommonMinimal文件夹下的crhook.c文件中有使用时间片钩子函数的例程。

### 7.configCPU_CLOCK_HZ
写入实际的CPU内核时钟频率，也就是CPU指令执行频率，通常称为Fcclk。配置此值是为了正确的配置系统节拍中断周期。

### 8.configTICK_RATE_HZ
RTOS 系统节拍中断的频率。即一秒中断的次数，每次中断RTOS都会进行任务调度。

系统节拍中断用来测量时间，因此，越高的测量频率意味着可测到越高的分辨率时间。但是，高的系统节拍中断频率也意味着RTOS内核占用更多的CPU时间，因此会降低效率。RTOS演示例程都是使用系统节拍中断频率为1000HZ，这是为了测试RTOS内核，比实际使用的要高。（实际使用时不用这么高的系统节拍中断频率）

多个任务可以共享一个优先级，RTOS调度器为相同优先级的任务分享CPU时间，在每一个RTOS 系统节拍中断到来时进行任务切换。高的系统节拍中断频率会降低分配给每一个任务的“时间片”持续时间。

### 9.configMAX_PRIORITIES
配置应用程序有效的优先级数目。任何数量的任务都可以共享一个优先级，使用协程可以单独的给与它们优先权。见configMAX_CO_ROUTINE_PRIORITIES。

在RTOS内核中，每个有效优先级都会消耗一定量的RAM，因此这个值不要超过你的应用实际需要的优先级数目。

注：<font face="仿宋" font color=#FF0000>任务优先级</font>

每一个任务都会被分配一个优先级，优先级值从0~ （configMAX_PRIORITIES - 1）之间。低优先级数表示低优先级任务。空闲任务的优先级为0（tskIDLE_PRIORITY），因此它是最低优先级任务。

FreeRTOS调度器将确保处于就绪状态（Ready）或运行状态（Running）的高优先级任务比同样处于就绪状态的低优先级任务优先获取处理器时间。换句话说，处于运行状态的任务永远是高优先级任务。

处于就绪状态的相同优先级任务使用时间片调度机制共享处理器时间。

### 10.configMINIMAL_STACK_SIZE
定义空闲任务使用的堆栈大小。通常此值不应小于对应处理器演示例程文FreeRTOSConfig.h中定义的数值。

就像xTaskCreate()函数的堆栈大小参数一样，堆栈大小不是以字节为单位而是以字为单位的，比如在32位架构下，栈大小为100表示栈内存占用400字节的空间。

### 11.configTOTAL_HEAP_SIZE
RTOS内核总计可用的有效的RAM大小。仅在你使用官方下载包中附带的内存分配策略时，才有可能用到此值。每当创建任务、队列、互斥量、软件定时器或信号量时，RTOS内核会为此分配RAM，这里的RAM都属于configTOTAL_HEAP_SIZE指定的内存区。后续的内存配置会详细讲到官方给出的内存分配策略。

### 12.configMAX_TASK_NAME_LEN
调用任务函数时，需要设置描述任务信息的字符串，这个宏用来定义该字符串的最大长度。这里定义的长度包括字符串结束符’’。

### 13.configUSE_TRACE_FACILITY
设置成1表示启动可视化跟踪调试，会激活一些附加的结构体成员和函数。

### 14.configUSE_STATS_FORMATTING_FUNCTIONS （V7.5.0新增）
设置宏configUSE_TRACE_FACILITY和configUSE_STATS_FORMATTING_FUNCTIONS为1会编译vTaskList()和vTaskGetRunTimeStats()函数。如果将这两个宏任意一个设置为0，上述两个函数不会被编译。

### 15.configUSE_16_BIT_TICKS
定义系统节拍计数器的变量类型，即定义portTickType是表示16位变量还是32位变量。

定义configUSE_16_BIT_TICKS为1意味着portTickType代表16位无符号整形，定义configUSE_16_BIT_TICKS为0意味着portTickType代表32位无符号整形。

使用16位类型可以大大提高8位和16位架构微处理器的性能，但这也限制了最大时钟计数为65535个’Tick’。因此，如果Tick频率为250HZ（4MS中断一次），对于任务最大延时或阻塞时间，16位计数器是262秒，而32位是17179869秒。

### 16.configIDLE_SHOULD_YIELD
这个参数控制任务在空闲优先级中的行为。仅在满足下列条件后，才会起作用。

使用抢占式内核调度

用户任务使用空闲优先级。

通过时间片共享同一个优先级的多个任务，如果共享的优先级大于空闲优先级，并假设没有更高优先级任务，这些任务应该获得相同的处理器时间。

但如果共享空闲优先级时，情况会稍微有些不同。当configIDLE_SHOULD_YIELD为1时，其它共享空闲优先级的用户任务就绪时，空闲任务立刻让出CPU，用户任务运行，这样确保了能最快响应用户任务。处于这种模式下也会有不良效果（取决于你的程序需要），描述如下：

图中描述了四个处于空闲优先级的任务，任务A、B和C是用户任务，任务I是空闲任务。上下文切换周期性的发生在T0、T1…T6时刻。当用户任务运行时，空闲任务立刻让出CPU，但是，空闲任务已经消耗了当前时间片中的一定时间。这样的结果就是空闲任务I和用户任务A共享一个时间片。用户任务B和用户任务C因此获得了比用户任务A更多的处理器时间。

可以通过下面方法避免：

如果合适的话，将处于空闲优先级的各单独的任务放置到空闲钩子函数中；

创建的用户任务优先级大于空闲优先级；

设置IDLE_SHOULD_YIELD为0；

设置configIDLE_SHOULD_YIELD为0将阻止空闲任务为用户任务让出CPU，直到空闲任务的时间片结束。这确保所有处在空闲优先级的任务分配到相同多的处理器时间，但是，这是以分配给空闲任务更高比例的处理器时间为代价的。

### 17.configUSE_TASK_NOTIFICATIONS（V8.2.0新增）
设置宏configUSE_TASK_NOTIFICATIONS为1（或不定义宏configUSE_TASK_NOTIFICATIONS）将会开启任务通知功能，有关的API函数也会被编译。设置宏configUSE_TASK_NOTIFICATIONS为0则关闭任务通知功能，相关API函数也不会被编译。默认这个功能是开启的。开启后，每个任务多增加8字节RAM。

这是个很有用的特性，一大亮点。

每个RTOS任务具有一个32位的通知值，RTOS任务通知相当于直接向任务发送一个事件，接收到通知的任务可以解除任务的阻塞状态（因等待任务通知而进入阻塞状态）。相对于以前必须分别创建队列、二进制信号量、计数信号量或事件组的情况，使用任务通知显然更灵活。更好的是，相比于使用信号量解除任务阻塞，使用任务通知可以快45%（使用GCC编译器，-o2优化级别）。

### 18.configUSE_MUTEXES
设置为1表示使用互斥量，设置成0表示忽略互斥量。读者应该了解在FreeRTOS中互斥量和二进制信号量的区别。

关于互斥量和二进制信号量简单说：

互斥型信号量必须是同一个任务申请，同一个任务释放，其他任务释放无效。

二进制信号量，一个任务申请成功后，可以由另一个任务释放。

互斥型信号量是二进制信号量的子集

### 19.configUSE_RECURSIVE_MUTEXES
设置成1表示使用递归互斥量，设置成0表示不使用。

### 20.configUSE_COUNTING_SEMAPHORES
设置成1表示使用计数信号量，设置成0表示不使用。

### 21.configUSE_ALTERNATIVE_API
设置成1表示使用“替代”队列函数（'alternative' queue functions），设置成0不使用。替代API在queue.h头文件中有详细描述。

注：“替代”队列函数已经被弃用，在新的设计中不要使用它！

### 22.configCHECK_FOR_STACK_OVERFLOW
每个任务维护自己的栈空间，任务创建时会自动分配任务需要的占内存，分配内存大小由创建任务函数（xTaskCreate()）的一个参数指定。堆栈溢出是设备运行不稳定的最常见原因，因此FreeeRTOS提供了两个可选机制用来辅助检测和改正堆栈溢出。配置宏configCHECK_FOR_STACK_OVERFLOW为不同的常量来使用不同堆栈溢出检测机制。

注意，这个选项仅适用于内存映射未分段的微处理器架构。并且，在RTOS检测到堆栈溢出发生之前，一些处理器可能先产生故障（fault）或异常（exception）来反映堆栈使用的恶化。如果宏configCHECK_FOR_STACK_OVERFLOW没有设置成0，用户必须提供一个栈溢出钩子函数，这个钩子函数的函数名和参数必须如下所示：

`void vApplicationStackOverflowHook(TaskHandle_t xTask, signed char *pcTaskName ); ` 
参数xTask和pcTaskName为堆栈溢出任务的句柄和名字。请注意，如果溢出非常严重，这两个参数信息也可能是错误的！在这种情况下，可以直接检查pxCurrentTCb变量。

推荐仅在开发或测试阶段使用栈溢出检查，因为堆栈溢出检测会增大上下文切换开销。

任务切换出去后，该任务的上下文环境被保存到自己的堆栈空间，这时很可能堆栈的使用量达到了最大（最深）值。在这个时候，RTOS内核会检测堆栈指针是否还指向有效的堆栈空间。如果堆栈指针指向了有效堆栈空间之外的地方，堆栈溢出钩子函数会被调用。

这个方法速度很快，但是不能检测到所有堆栈溢出情况（比如，堆栈溢出没有发生在上下文切换时）。设置configCHECK_FOR_STACK_OVERFLOW为1会使用这种方法。

当堆栈首次创建时，在它的堆栈区中填充一些已知值（标记）。当任务切换时，RTOS内核会检测堆栈最后的16个字节，确保标记数据没有被覆盖。如果这16个字节有任何一个被改变，则调用堆栈溢出钩子函数。

这个方法比第一种方法要慢，但也相当快了。它能有效捕捉堆栈溢出事件（即使堆栈溢出没有发生在上下文切换时），但是理论上它也不能百分百的捕捉到所有堆栈溢出（比如堆栈溢出的值和标记值相同，当然，这种情况发生的概率极小）。

使用这个方法需要设置configCHECK_FOR_STACK_OVERFLOW为2.

### 23.configQUEUE_REGISTRY_SIZE
队列记录有两个目的，都涉及到RTOS内核的调试：

它允许在调试GUI中使用一个队列的文本名称来简单识别队列；

包含调试器需要的每一个记录队列和信号量定位信息；

除了进行内核调试外，队列记录没有其它任何目的。

configQUEUE_REGISTRY_SIZE定义可以记录的队列和信号量的最大数目。如果你想使用RTOS内核调试器查看队列和信号量信息，则必须先将这些队列和信号量进行注册，只有注册后的队列和信号量才可以使用RTOS内核调试器查看。查看API参考手册中的vQueueAddToRegistry() 和vQueueUnregisterQueue()函数获取更多信息。

### 24.configUSE_QUEUE_SETS
设置成1使能队列集功能（可以阻塞、挂起到多个队列和信号量），设置成0取消队列集功能。

### 25.configUSE_TIME_SLICING（V7.5.0新增）
默认情况下（宏configUSE_TIME_SLICING未定义或者宏configUSE_TIME_SLICING设置为1），FreeRTOS使用基于时间片的优先级抢占式调度器。这意味着RTOS调度器总是运行处于最高优先级的就绪任务，在每个RTOS 系统节拍中断时在相同优先级的多个任务间进行任务切换。如果宏configUSE_TIME_SLICING设置为0，RTOS调度器仍然总是运行处于最高优先级的就绪任务，但是当RTOS 系统节拍中断发生时，相同优先级的多个任务之间不再进行任务切换。

### 26.configUSE_NEWLIB_REENTRANT（V7.5.0新增）
 如果宏configUSE_NEWLIB_REENTRANT设置为1，每一个创建的任务会分配一个newlib（一个嵌入式C库）reent结构。

### 27.configENABLE_BACKWARD_COMPATIBILITY
头文件FreeRTOS.h包含一系列#define宏定义，用来映射版本V8.0.0和V8.0.0之前版本的数据类型名字。这些宏可以确保RTOS内核升级到V8.0.0或以上版本时，之前的应用代码不用做任何修改。在FreeRTOSConfig.h文件中设置宏configENABLE_BACKWARD_COMPATIBILITY为0会去掉这些宏定义，并且需要用户确认升级之前的应用没有用到这些名字。

### 28.configNUM_THREAD_LOCAL_STORAGE_POINTERS
设置每个任务的线程本地存储指针数组大小。

线程本地存储允许应用程序在任务的控制块中存储一些值，每个任务都有自己独立的储存空间，宏configNUM_THREAD_LOCAL_STORAGE_POINTERS指定每个任务线程本地存储指针数组的大小。API函数vTaskSetThreadLocalStoragePointer()用于向指针数组中写入值，API函数pvTaskGetThreadLocalStoragePointer()用于从指针数组中读取值。

比如，许多库函数都包含一个叫做errno的全局变量。某些库函数使用errno返回库函数错误信息，应用程序检查这个全局变量来确定发生了那些错误。在单线程程序中，将errno定义成全局变量是可以的，但是在多线程应用中，每个线程（任务）必须具有自己独有的errno值，否则，一个任务可能会读取到另一个任务的errno值。

FreeRTOS提供了一个灵活的机制，使得应用程序可以使用线程本地存储指针来读写线程本地存储。具体参见后续文章《FreeRTOS系列第12篇---FreeRTOS任务应用函数》。

### 29.configGENERATE_RUN_TIME_STATS
设置宏configGENERATE_RUN_TIME_STATS为1使能运行时间统计功能。一旦设置为1，则下面两个宏必须被定义：

portCONFIGURE_TIMER_FOR_RUN_TIME_STATS()：用户程序需要提供一个基准时钟函数，函数完成初始化基准时钟功能，这个函数要被define到宏portCONFIGURE_TIMER_FOR_RUN_TIME_STATS()上。这是因为运行时间统计需要一个比系统节拍中断频率还要高分辨率的基准定时器，否则，统计可能不精确。基准定时器中断频率要比统节拍中断快10~100倍。基准定时器中断频率越快，统计越精准，但能统计的运行时间也越短（比如，基准定时器10ms中断一次，8位无符号整形变量可以计到2.55秒，但如果是1秒中断一次，8位无符号整形变量可以统计到255秒）。

portGET_RUN_TIME_COUNTER_VALUE()：用户程序需要提供一个返回基准时钟当前“时间”的函数，这个函数要被define到宏portGET_RUN_TIME_COUNTER_VALUE()上。

举一个例子，假如我们配置了一个定时器，每500us中断一次。在定时器中断服务例程中简单的使长整形变量ulHighFrequencyTimerTicks自增。那么上面提到两个宏定义如下（可以在FreeRTOSConfig.h中添加）：

```
extern volatile unsigned longulHighFrequencyTimerTicks;  
#define portCONFIGURE_TIMER_FOR_RUN_TIME_STATS() ( ulHighFrequencyTimerTicks = 0UL )  
#define portGET_RUN_TIME_COUNTER_VALUE() ulHighFrequencyTimerTicks 
```

### 30.configUSE_CO_ROUTINES
设置成1表示使用协程，0表示不使用协程。如果使用协程，必须在工程中包含croutine.c文件。

注：协程（Co-routines）主要用于资源发非常受限的嵌入式系统（RAM非常少），通常不会用于32位微处理器。

在当前嵌入式硬件环境下，不建议使用协程，FreeRTOS的开发者早已经停止开发协程。

### 31.configMAX_CO_ROUTINE_PRIORITIES
应用程序协程（Co-routines）的有效优先级数目，任何数目的协程都可以共享一个优先级。使用协程可以单独的分配给任务优先级。见configMAX_PRIORITIES。

### 32.configUSE_TIMERS
设置成1使用软件定时器，为0不使用软件定时器功能。详细描述见FreeRTOS software timers 。

### 33.configTIMER_TASK_PRIORITY
设置软件定时器服务/守护进程的优先级。详细描述见FreeRTOS software timers 。

### 34.configTIMER_QUEUE_LENGTH
设置软件定时器命令队列的长度。详细描述见FreeRTOS software timers。

### 35.configTIMER_TASK_STACK_DEPTH
设置软件定时器服务/守护进程任务的堆栈深度，详细描述见FreeRTOS software timers 。

### 36.configKERNEL_INTERRUPT_PRIORITY、configMAX_SYSCALL_INTERRUPT_PRIORITY和configMAX_API_CALL_INTERRUPT_PRIORITY
这是移植和应用FreeRTOS出错最多的地方。

Cortex-M3、PIC24、dsPIC、PIC32、SuperH和RX600硬件设备需要设置宏configKERNEL_INTERRUPT_PRIORITY；PIC32、RX600和Cortex-M硬件设备需要设置宏configMAX_SYSCALL_INTERRUPT_PRIORITY。

configMAX_SYSCALL_INTERRUPT_PRIORITY、configMAX_API_CALL_INTERRUPT_PRIORITY，这两个宏是等价的，后者是前者的新名字，用于更新的移植层代码。

注意下面的描述中，在中断服务例程中仅可以调用以“FromISR”结尾的API函数。

仅需要设置configKERNEL_INTERRUPT_PRIORITY的硬件设备（也就是宏configMAX_SYSCALL_INTERRUPT_PRIORITY不会用到）：configKERNEL_INTERRUPT_PRIORITY用来设置RTOS内核自己的中断优先级。调用API函数的中断必须运行在这个优先级；不调用API函数的中断，可以运行在更高的优先级，所以这些中断不会被因RTOS内核活动而延时。 

configKERNEL_INTERRUPT_PRIORITY和configMAX_SYSCALL_INTERRUPT_PRIORITY都需要设置的硬件设备：configKERNEL_INTERRUPT_PRIORITY用来设置RTOS内核自己的中断优先级。因为RTOS内核中断不允许抢占用户使用的中断，因此这个宏一般定义为硬件最低优先级。configMAX_SYSCALL_INTERRUPT_PRIORITY用来设置可以在中断服务程序中安全调用FreeRTOS API函数的最高中断优先级。优先级小于等于这个宏所代表的优先级时，程序可以在中断服务程序中安全的调用FreeRTOS API函数；如果优先级大于这个宏所代表的优先级，表示FreeRTOS无法禁止这个中断，在这个中断服务程序中绝不可以调用任何API函数。

通过设置configMAX_SYSCALL_INTERRUPT_PRIORITY的优先级级别高于configKERNEL_INTERRUPT_PRIORITY可以实现完整的中断嵌套模式。这意味着FreeRTOS内核不能完全禁止中断，即使在临界区。此外，这对于分段内核架构的微处理器是有利的。请注意，当一个新中断发生后，某些微处理器架构会（在硬件上）禁止中断，这意味着从硬件响应中断到FreeRTOS重新使能中断之间的这段短时间内，中断是不可避免的被禁止的。

不调用API的中断可以运行在比configMAX_SYSCALL_INTERRUPT_PRIORITY高的优先级，这些级别的中断不会被FreeRTOS禁止，因此不会因为执行RTOS内核而被延时。

这些配置参数允许非常灵活的中断处理：

在系统中可以像其它任务一样为中断处理任务分配优先级。这些任务通过一个相应中断唤醒。中断服务例程（ISR）内容应尽可能的精简---仅用于更新数据然后唤醒高优先级任务。ISR退出后，直接运行被唤醒的任务，因此中断处理（根据中断获取的数据来进行的相应处理）在时间上是连续的，就像ISR在完成这些工作。这样做的好处是当中断处理任务执行时，所有中断都可以处在使能状态。

中断、中断服务例程（ISR）和中断处理任务是三码事：当中断来临时会进入中断服务例程，中断服务例程做必要的数据收集（更新），之后唤醒高优先级任务。这个高优先级任务在中断服务例程结束后立即执行，它可能是其它任务也可能是中断处理任务，如果是中断处理任务，那么就可以根据中断服务例程中收集的数据做相应处理。

configMAX_SYSCALL_INTERRUPT_PRIORITY接口有着更深一层的意义：在优先级介于RTOS内核中断优先级（等于configKERNEL_INTERRUPT_PRIORITY）和configMAX_SYSCALL_INTERRUPT_PRIORITY之间的中断允许全嵌套中断模式并允许调用API函数。大于configMAX_SYSCALL_INTERRUPT_PRIORITY的中断优先级绝不会因为执行RTOS内核而延时。

运行在大于configMAX_SYSCALL_INTERRUPT_PRIORITY的优先级中断是不会被RTOS内核所屏蔽的，因此也不受RTOS内核功能影响。这主要用于非常高的实时需求中。比如执行电机转向。但是，这类中断的中断服务例程中绝不可以调用FreeRTOS的API函数。

为了使用这个方案，应用程序要必须符合以下规则：调用FreeRTOS API函数的任何中断，都必须和RTOS内核处于同一优先级（由宏configKERNEL_INTERRUPT_PRIORITY设置），或者小于等于宏configMAX_SYSCALL_INTERRUPT_PRIORITY定义的优先级。

### 37.configASSERT
断言，调试时可以检查传入的参数是否合法。FreeRTOS内核代码的关键点都会调用configASSERT( x )函数，如果参数x为0，则会抛出一个错误。这个错误很可能是传递给FreeRTOS API函数的无效参数引起的。定义configASSERT()有助于调试时发现错误，但是，定义configASSERT()也会增大应用程序代码量，增大运行时间。推荐在开发阶段使用这个断言宏。

举一个例子，我们想把非法参数所在的文件名和代码行数打印出来，可以先定义一个函数vAssertCalled，该函数有两个参数，分别接收触发configASSERT宏的文件名和该宏所在行，然后通过显示屏或者串口输出。代码如下：

#define configASSERT( ( x ) )     if( ( x ) == 0 )vAssertCalled( __FILE__, __LINE__ )
这里__FILE__和__LINE__是大多数编译器预定义的宏，分别表示代码所在的文件名（字符串格式）和行数（整形）。

这个例子虽然看起来很简单，但由于要把整形__LINE__转换成字符串再显示，在效率和实现上，都不能让人满意。我们可以使用C标准库assert的实现方法，这样函数vAssertCalled只需要接收一个字符串形式的参数（推荐仔细研读下面的代码并理解其中的技巧）：

#define STR(x)  VAL(x)  
#define VAL(x)  #x  
#define configASSERT(x) ((x)?(void) 0 :xAssertCalld(__FILE__ ":" STR(__LINE__) " " #x"
"))  
这里稍微讲解一下，由于内置宏__LINE__是整数型的而不是字符串型，把它转化成字符串需要一个额外的处理层。宏STR和和宏VAL正是用来辅助完成这个转化。宏STR用来把整形行号替换掉__LINE__，宏VAL用来把这个整形行号字符串化。忽略宏STR和VAL中的任何一个，只能得到字符串”__LINE__”，这不是我们想要的。

这里使用三目运算符’?:’来代替参数判断if语句，这样可以接受任何参数或表达式，代码也更紧凑，更重要的是代码优化度更高，因为如果参数恒为真，在编译阶段就可以去掉不必要的输出语句。

### 38.INCLUDE Parameters
以“INCLUDE”起始的宏允许用户不编译那些应用程序不需要的实时内核组件（函数），这可以确保在你的嵌入式系统中RTOS占用最少的ROM和RAM。

每个宏以这样的形式出现：

INCLUDE_FunctionName

在这里FunctionName表示一个你可以控制是否编译的API函数。如果你想使用该函数，就将这个宏设置成1，如果不想使用，就将这个宏设置成0。比如，对于API函数vTaskDelete()：

`#define INCLUDE_vTaskDelete    1  `
     表示希望使用vTaskDelete()，允许编译器编译该函数

`#define INCLUDE_vTaskDelete    0  `
  表示希望禁止vTaskDelete()，禁止编译器编译该函数。

## 文章来源；
https://blog.csdn.net/freestep96/article/details/126674642