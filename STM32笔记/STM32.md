# GPIO口简介 #  
![GPIO基本结构](img/GPIO基本结构.png)  
每个GPIO外设总共有十五个引脚编号是0到15。  
*寄存器*： 一种特殊的存储器，内核通过APB2总线对寄存器读写，完成输出电平和读取电平的功能。  
1.寄存器的每一位对应一个引脚输出寄存器写入1输出高电平写入0输入低电平。输入寄存器读取为1就为高电平读取为0就为低电平。  
*驱动器*： 增加信号驱动力  
![GPIO位结构](img/GPIO位结构.png)  
 对GPIO的初始化   
 `RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOx, ENABLE);`打开GPIO时钟  
 `GPIO_InitTypeDef GPIO_InitStructure;`定义GPIO结构体    
 `GPIO_InitStructure.GPIO_Pin = GPIO_Pin_x;`选择引脚  
 `GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;`设置输出模式   
 `GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;`设置频率  
 `GPIO_Init(GPIOx, &GPIO_InitStructure);`   
肖特基触发器改为施密特触发器（翻译错误）作用：对输入电压进行整形。  jegou
**通用输入输出口**  
一.八种输入输出模式  
![GPIO模式](img/八种输入输出模式.png)  
1.浮空/上拉/下拉输入。  
![浮空/上拉/下拉配置](img/浮空上拉下拉.png)  
输入模式下输出驱动器断开,端口只能输入不能输出。  
2.模拟输入。  
![模拟输入](img/模拟输入.png)  
输出驱动器断开,端口只能输入不能输出。 
输入的施密特触发器断开也是无效状态。  
3.开漏输出和推挽输出。  
![开漏输出和推挽输出](img/开漏输出和推挽输出.png)  
输出模式下输入模式也是有效的。  
原因：一个端口只能有一个输出但可以有多个输入。  
4.复用开漏/推挽输出。  
![复用开漏推挽输出](img/复用开漏推挽输出.png)  
通用输出未连接，引脚由片上外设控制。  
# LED和蜂鸣器 #  
*LED*  
发光二极管，正向通电点亮反向通电不亮。 
长脚是正极短脚是负极   
低电平驱动点亮  
![低电平](img/低电平.png)  
高电平驱动点亮  
![高电平](img/高电平.png)   
*蜂鸣器*  
1.有源蜂鸣器：内部自带震荡源，将正负极接上直流电压即可发生频率固定。  
2.无源蜂鸣器：内部不带震荡源，需要控制器提供震荡脉冲才能发声，调整提供震荡脉冲的频率，可发出不同的声音。   
# 调试方式 #   
1.串口调试：通过串口通信，将调试信息发送到电脑电脑端，电脑使用串口助手显示调试信息。  
2.显示屏调试：直接将显示屏连接到单片机，将调试信息打印在显示屏上。  
3.Keli调试模式:借助Keil软件的调试模式,可使用单步运行，设置断点，查看寄存器及变量等功能。  
# 中断系统 #  
*中断*:在主程序运行过程中,出现了特定的中断触发条件(中断源),使得CPU暂停当前正在被运行的程序,转而去处理中断程序，处理完成后又返回原来被暂停的位置继续执行。  
*中断优先级*:当有多个中断源同时申请中断时,CPU会根据中断源的轻重缓急进行裁决,优先响应更加紧急的中断源。  
*中断嵌套*:当有一个中断程序正在运行时,又有新的更高优先级的中断源申请中断,CPU再次暂停当前中断程序,转而去处理新的中断程序，处理完成后再以此进行返回。  
NVIC基本结构
![NVIC基本结构](img/NVIC基本结构.png)  
负责中断优先级的分配任务  
NVIC优先级分组  
1.抢占优先级：终止当前运行的程序马上执行中断(中断嵌套)。  
2.响应优先级：运行完当前程序再执行中断(优先排队)。   
![NVIC优先级分组](img/NVIC优先级分组.png)  
初始化NVIC   
`NVIC_InitTypeDef NVIC_InitStructure;`定义NVIC结构体   
`NVIC_InitStructure.NVIC_IRQChannel = IRQChannelx;`   
`NVIC_InitStructure.NVIC_IRQChannelPreemptionPriority = 0;`抢占优先级   
`NVIC_InitStructure.NVIC_IRQChannelSubPriority = 0;`中断优先级   
`NVIC_Init(&NVIC_InitStructure);`  
ESTI介绍  
![ESTI](img/ESTI.png)  
ESTI基本结构  
![ESTI基本结构](img/ESTI基本结构.png) 
初始化EXTI   
`EXTI_InitTypeDef EXTI_InitStructure;`定义EXTI结构体  
`EXTI_InitStructure.EXTI_Line = EXTI_Linex;`  
`EXTI_InitStructure.EXTI_Mode = EXTI_Mode_Interrupt;`  
`EXTI_InitStructure.EXTI_Trigger = EXTI_Trigger_Rising;`   
`EXTI_InitStructure.EXTI_LineCmd = ENABLE;`启用EXTI   
`EXTI_Init(&EXTI_InitStructure);`
# 定时器 #
TIM简介  
![TIM简介](img/TIM简介.png)  
定时器类型   
![定时器类型](img/定时器类型.png)  
基本定时器结构  
![基本定时器](img/基本定时器.png)  
*时基单元* :  
1.预分频器:  
写0为一分频输出频率 = 72MHz/1  
写1为二分频输出频率 = 72MHz/2   
......   
实际分频系数 = 预分频器的值 + 1  
2.计数器 (只支持向上计数):  
对预分频后的计数时钟进行计数  
计数值不断自增  
3.自动重装寄存器:   
存入我们写入的计数目标  
计数值 = 自动重装值时即计时时间到，同时清零计数器  
*主模式触发DAC*(STM32定时器特色) :  
让内部硬件在不受程序控制下实现自动运行减轻CPU负担  
将定时器的更新事件映射到触发输出TRGO的位置，TRGO直接接DAC的触发转换引脚  
通用定时器结构  
![通用定时器](img/通用定时器.png)  
计数器:  
计数器计数模式  
1.向上计数:从零开始向上自增，自增到重装值申请中断清零重新自增。  
2.向下计数:从重装值开始自减，自减到零申请中断回到重装值重新自减。  
3.中央对其:从零开始自增到重装值申请中断再从重装值自减到零申请中断。  
定时中断基本结构  
![定时中断基本结构](img/定时中断基本结构.png)   
预分频器时序  
![预分频器时序](img/预分频器时序.png)  
计数器时序  
![计数器时序](img/计数器时序.png)  
计数器有预装时序  
![计数器有预装时序](img/计数器有预装时序.png)  
计数器无预装时序  
![计数器无预装时序](img/计数器无预装时序.png)   
对比:无预装时序时当突然更改自动重装寄存器后若此时计数器寄存器的值已经比更改后的值大计数器寄存器将会一直自增直到FFFF才会再回到零然后再自增到改后的值产生更新。  
有预装时序时更改自动重装寄存器后自动加载影子寄存器会结束完上一次的自增后在下一个周期更改值才有效。 
初始化定时器   
`RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM2, ENABLE);`打开定时器时钟    
`TIM_TimeBaseInitTypeDef TIM_TimeBaseStructure;`定义定时器结构体  
`TIM_TimeBaseStructure.TIM_Prescaler = 71;`预分频器   
`TIM_TimeBaseStructure.TIM_CounterMode = TIM_CounterMode_Up;`向上计数   
`TIM_TimeBaseStructure.TIM_Period = 10000;`自动重装值  
`TIM_TimeBaseStructure.TIM_ClockDivision = TIM_CKD_DIV1;`时钟分频  
`TIM_TimeBaseInit(TIM2, &TIM_TimeBaseStructure);`初始化定时器     
RCC时钟树  
![RCC时钟树](img/RCC时钟树.png)  
# TIM输出比较 #  
输出比较简介  
![输出比较](img/输出比较.png)  
OC:输出比较  
IC(Input Capture):输入捕获  
CC(Capture/Compare):输入捕获和输出比较  
PWM简介  
![](img/PWM简介.png)  
快速的调高低电平以实现LED的不同程度的亮度,电机调速的不同速度等  
使用PWM波形即可在数字系统等效输出模拟量   
 ![](img/输出比较通道.png)  
CNT计数器  
CCR捕获比较寄存器  
![](img/输出比较模式.png)  
高级定时器中的说法:
置有效电平,置无效电平  
理解为置有效电平就是置高电平,置无效电平就是置低电平  
PWM基本结构(**重点**)  
![](img/PWM基本结构.png)  
PWM初始化   
`TIM_OCInitTypeDef TIM_OCInitStructure;`定义输出比较结构体  
`TIM_OCInitStructure.TIM_OCMode = TIM_OCMode_PWM1;`PWM1模式   
`TIM_OCInitStructure.TIM_OutputState = TIM_OutputState_Enable;`输出使能    
`TIM_OCInitStructure.TIM_Pulse = 5000;`脉宽  
`TIM_OCInitStructure.TIM_OCPolarity = TIM_OCPolarity_High;`高电平有效  
`TIM_OC1Init(TIM2, &TIM_OCInitStructure);`初始化输出比较     
参数计算  
![](img/参数计算.png)  
详细解释   
ARR：自动重装值   
当计数器达到ARR时产生更新，同时清零计数器  
PSC：预分频器  
对输入的时钟进行分频   
输入时钟频率/(PSC+1) = 输出时钟频率  
CCR：捕获/比较寄存器   
当计数器的值等于CCR值时，输出引脚状态改变  
舵机简介   
![](img/舵机简介.png)   
直流电机及驱动简介   
![](img/直流电机.png)  
# TIM输入捕获 #
输入捕获简介   
![输入捕获](img/输入捕获.png)   
输入捕获初始化   
`TIM_ICInitTypeDef TIM_ICInitStructure;`定义输入捕获结构体   
`TIM_ICInitStructure.TIM_Channel = TIM_Channel_1;`输入捕获通道   
`TIM_ICInitStructure.TIM_ICPolarity = TIM_ICPolarity_Rising;`上升沿有效   
`TIM_ICInitStructure.TIM_ICSelection = TIM_ICSelection_DirectTI;`直接TI模式   
`TIM_ICInitStructure.TIM_ICPrescaler = TIM_ICPSC_DIV1;`输入捕获分频   
`TIM_ICInitStructure.TIM_ICFilter = 0x0;`输入捕获滤波器   
`TIM_ICInit(TIM3, &TIM_ICInitStructure);`初始化输入捕获   
频率测量  
![频率测量](img/频率测量.png)   
输入捕获基本结构   
![输入捕获基本结构](img/输入捕获基本结构.png)   
配置从模式为Reset      
`TIM_SelectInputTrigger(TIM3, TIM_TS_TI1FP1);`  
`TIM_SelectSlaveMode(TIM3, TIM_SlaveMode_Reset);`  
PWMI基本结构  
![PWMI基本结构](img/PWMI基本结构.png)   
`TIM_PWMIConfing(TIM3,&TIM_ICInitStructure);`自动把另一个通道初始化为相反配置  
# 编码器接口 #
编码器接口简介  
![编码器接口](img/编码器接口.png)   
正交编码器   
![正交编码器](img/正交编码器.png)  
旋转编码器  
![旋转编码器](img/旋转编码器.png)   
编码器接口基本结构  
![编码器接口基本结构](img/编码器接口基本结构.png)    
ARR设置为65535   
正转CNT自增0,1,2.......65535   
反转CNT自减65535,65534,65533......0  
将该16位无符号数转换为16位有符号数即65535对应-1......以此读取出负数   
工作模式  
![工作模式](img/工作模式.png)  
前两种模式计数精度低  
实例  
![实例](img/实例.png)    
反相实例  
![反相实例](img/反相实例.png)  
TI1反相:反转TI1的高低电平(在边沿选择极性选择处加非门反转极性)  
# ADC模数转换器 #
ADC简介   
![ADC简介](img/ADC简介.png)  
逐次逼近型ADC   
![逐次逼近型ADC](img/逐次逼近型ADC.png)   
ADC框图   
![ADC框图](img/ADC框图.png)   
ADC基本结构   
![ADC基本结构](img/ADC基本结构.png)  
输入通道(对应的GPIO口)  
![输入通道](img/输入通道.png)  
规则通道  
转换模式1  
![转换模式1](img/转换模式1.png)  
每触发一次后转换会停下来  
只有第一个序列一位置有效退化为简单选择一个的方式  
转换模式2  
![转换模式2](img/转换模式2.png)   
一次转换后不会停止而是立刻开启下一轮转换(只需最开始触发一次)  
转换模式3  
![转换模式3](img/转换模式3.png)  
每触发一次后转换会停下来  
依次对前七个位置进行转换结束后产生EOC信号转换结束  
转换模式4   
![转换模式4](img/转换模式4.png)   
一次转换完成后不会停止而是立刻开启下一轮转换(只需最开始触发一次)
触发控制  
![触发控制](img/触发控制.png)   
规则组触发源  
数据对齐  
![数据对齐](img/数据对齐.png)  
一般右对齐  
初始化ADC   
`RCC_ADCCLKConfig(RCC_PCLK2_Div8);`设置时钟分频   
`RCC_APB2PeriphClockCmd(RCC_APB2Periph_ADC1, ENABLE);`打开ADC时钟   
`ADC_InitTypeDef ADC_InitStructure;`定义ADC结构体   
`ADC_InitStructure.ADC_Mode = ADC_Mode_Independent;`独立模式   
`ADC_InitStructure.ADC_ScanConvMode = ENABLE;`扫描模式    
`ADC_InitStructure.ADC_ContinuousConvMode = ENABLE;`连续转换模式    
`ADC_InitStructure.ADC_ExternalTrigConv = ADC_ExternalTrigConv_None;`外部触发源      
`ADC_InitStructure.ADC_DataAlign = ADC_DataAlign_Right;`数据对齐     
`ADC_InitStructure.ADC_NbrOfChannel = 1;`通道数    
`ADC_Init(ADC1, &ADC_InitStructure);`初始化ADC    
转换时间  
![转换时间](img/转换时间.png)  
校准  
![校准](img/校准.png)  
ADC校准   
`ADC_ResetCalibration(ADC1);`重置校准   
`ADC_StartCalibration(ADC1);`开始校准   
`while(ADC_GetCalibrationStatus(ADC1));`等待校准结束  
ADC转换  
`ADC_RegularChannelConfig(ADC1, ADC_Channel_1, 1, ADC_SampleTime_55Cycles5);`配置通道1   
`ADC_SoftwareStartConvCmd(ADC1, ENABLE);`开始转换    
`while(ADC_GetFlagStatus(ADC1, ADC_FLAG_EOC) == RESET);`等待转换结束    
`ADC_GetConversionValue(ADC1);`获取转换值   
硬件电路  
![硬件电路](img/硬件电路.png)   
DMA简介   
![DMA简介](img/DMA简介.png)   
存储器映像   
![存储器映像](img/存储器映像.png)   
ROM只读存储器是一种非易失性掉电不丢失的寄存器   
REM是随机存储器是一种易失性掉电丢失的寄存器  
DMA框图   
![DMA框图](img/DMA框图.png)    
DMA基本结构   
![DMA基本结构](img/DMA基本结构.png)   
DMA转运的条件  
1.开关控制DMA_Cmd必须使能   
2.传输计数器必须大于零   
3.触发源(必须有触发信号)   
DMA请求   
![DMA请求](img/DMA请求.png)   
表示基本结构中的触发部分   
输出宽度与对齐  
![输出宽度与对齐](img/输出宽度与对齐.png)    
源端宽度小于目标宽度时补零   
源端宽度大于目标宽度时舍弃高位  
数据转运+DMA   
![数据转运+DMA](img/数据转运+DMA.png)   
ADC扫描模式+DMA   
![ADC扫描模式+DMA](img/ADC扫描模式+DMA.png)   
初始化DMA    
`RCC_AHBPeriphClockCmd(RCC_AHBPeriph_DMA1, ENABLE);`打开DMA时钟    
`DMA_InitTypeDef DMA_InitStructure;`定义DMA结构体  
`DMA_InitStructure.DMA_PeripheralBaseAddr = (uint32_t)ADC1_DR_Address;`外设基地址     
`DMA_InitStructure.DMA_MemoryBaseAddr = (uint32_t)&ADC_ConvertedValue;`内存基地址  
`DMA_InitStructure.DMA_DIR = DMA_DIR_PeripheralSRC;`外设到内存    
`DMA_InitStructure.DMA_BufferSize = 1;`缓冲区大小   
`DMA_InitStructure.DMA_PeripheralInc = DMA_PeripheralInc_Disable;`外设地址不增加    
`DMA_InitStructure.DMA_MemoryInc = DMA_MemoryInc_Enable;`内存地址增加    
`DMA_InitStructure.DMA_PeripheralDataSize = DMA_PeripheralDataSize_HalfWord;`外设数据宽度   
`DMA_InitStructure.DMA_MemoryDataSize = DMA_MemoryDataSize_HalfWord;`内存数据宽度    
`DMA_InitStructure.DMA_Mode = DMA_Mode_Circular;`循环模式     
`DMA_InitStructure.DMA_Priority = DMA_Priority_High;`优先级   
`DMA_InitStructure.DMA_M2M = DMA_M2M_Disable;`内存到内存  
`DMA_Init(DMA1_Channel1, &DMA_InitStructure);`初始化DMA  
DMA使能      
`DMA_Cmd(DMA1_Channel1, ENABLE);`使能DMA    
# USART串口 #    
通信接口   
![通信接口](img/通信接口.png)   
串口通信  
![串口通信](img/串口通信.png)      
串口硬件电路  
![串口硬件电路](img/串口硬件电路.png)   
电平标准   
![电平标准](img/电平标准.png)    
串口参数及时序   
![串口参数及时序](img/串口参数及时序.png)    
USART外设简介   
![USART外设](img/USART外设.png)   
USART基本结构   
![USART基本结构](img/USART基本结构.png)   
数据帧   
![数据帧](img/数据帧.png)  
# I2C通信 #   
I2C通信    
![I2C通信](img/I2C通信.png)   
I2C硬件电路  
一主多从模式   
![I2C硬件电路](img/I2C硬件电路.png)  
I2C时序基本单元    
起始条件终止条件   
![I2C时序基本单元](img/起始条件终止条件.png)   
SCL：迟到早退    
SDA：早出晚归   
发送一个字节    
![发送一个字节](img/发送一个字节.png)  
 低电平主机放数据高电平从机读数据
接收一个字节   
![接收一个字节](img/接收一个字节.png)  
  低电平从机放数据高电平主机读数据
发送应答接收应答  
![发送应答接收应答](img/发送应答接收应答.png)   
MPU6050简介    
![MPU6050简介](img/MPU6050简介.png)    
加速度计具有静态稳定性不具有动态稳定性(物体运动状态下不可以测量倾斜角度)支持物体静态时测倾斜角度或动态时求加速度   
陀螺仪具有动态稳定性不具有静态稳定性可以测量出芯片在xyz轴的角速度对角速度积分可以得到角度但在静止状态下由于噪声的影响积分累积导致求出角度发生漂移即不具静态稳定性的原因    
MPU6050参数    
![MPU6050参数](img/MPU6050参数.png)   
MPU6050硬件电路    
![MPU6050硬件电路](img/MPU6050硬件电路.png)    
XCL和XDA可外接电磁传感器和气压传感器   
MPU6050框图    
![MPU6050框图](img/MPU6050框图.png)   






  











 




































