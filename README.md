# asm9260t
用于国产ARM9芯片ASM9260T的开源项目。包含RTOS，TCP/IP，GUI等常用组件。 加入QQ群：298708237，交流ASM9260T开发经验。

集成开发环境：IAR EWARM 6.40.5 或更高的兼容版本。

这是ASM9260T的ucos2.92完整代码，用Timer0的通道0作为系统时基，1ms每tick。
在usrentry.c文件中有void UserEntryInit(void)函数和void UserEntryLoop(void)：
UserEntryInit用于创建其它线程；
UserEntryLoop是最高优先级线程的循环，不应该返回。如果你看不惯我的框架组织方式，你就从frmentry.c动手，
这里面有main函数，是你C语言代码的开始，随便你发挥了。

程序定位在SDRAM的0x20008000地址处执行，定义在mmu.h文件中，并且和ld.icf保持一致；
我假设你已经有sysloader将demo-with-RTOS.bin从spi-flash中加载到SDRAM的0x20008000，
如果不是的话，你用j-link也可以直接调试，因为已经配置好了调试接口。

演示代码是每500ms将GPIO9_4管脚作为LED控制管脚进行反转，于是你看见1HZ的LED闪烁。你可以改写led.c文件自定义。

我要重点推荐的是我重新设计了icoll.c文件，作为中断向量的注册管理服务接口，效率是很高的，因为是查表而不是switch方式。

还有就是ucos-ii的这个移植是很高效的，没有冗余代码，运行在SVC模式。

项目的内存布局请参看"demo-with-RTOS\hw-cfg\ld.icf"文件。

希望这个项目模板对你有用，并且在使用后不要吝啬你的经验去帮助别人，共同营造ASM9260T的广阔应用天地。
我不是学生也不是微控制器新手更不是紫芯员工，我是想用这款芯片的企业里的老员工。
希望你也能尽量帮助别人，分享经验。大多数情况下这种分享并不是商业机密，除非你在骗自己。

如需交流，请加QQ群：298708237
你可以分发这个包给别人，但一定要留着QQ群的交流方式，以方便其他人。

我没有在每个代码文件里加入作者信息的习惯，请保留这个txt文件。

更新记录：

2016-03-01 加入emwin移植，演示视频的链接：
http://v.youku.com/v_show/id_XNzMzMDM2NDgw.html
使用ASM9260T的LCD控制器接口，LCD面板的分辨率是480*272.

2016-03-01 加入《sysloader-QSPI》项目，用于加载大尺寸应用程序或二次bootloader。
sysloader不能大于4KB，这是ASM9260T的SPI启动特性决定的。
sysloader会检查位于SPI-Flash中的程序镜像的CRC32码，如果正确则将其加载大SDRAM并运行。
具体工作流程和原理，请参考《ASM9260T笔记.pdf》文件（位于sysloader-QSPI\doc目录下）。
