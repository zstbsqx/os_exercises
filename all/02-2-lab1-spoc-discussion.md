# lab1 SPOC思考题

## 个人思考题

NOTICE
- 有"w2l2"标记的题是助教要提交到学堂在线上的。
- 有"w2l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。
---

请描述ucore OS配置和驱动外设时钟的准备工作包括哪些步骤？ (w2l2)
```
  + 采分点：说明了ucore OS在让外设时钟正常工作的主要准备工作
  - 答案没有涉及如下3点；（0分）
  - 描述了对IDT的初始化，包了针时钟中断的中断描述符的设置（1分）
  - 除第二点外，进一步描述了对8259中断控制器的初始过程（2分）
  - 除上述两点外，进一步描述了对8253时钟外设的初始化，或描述了对EFLAG操作使能中断（3分）
 ```
- [x]  

>  IDT初始化，设置时钟中断描述符。初始化8259中断控制器上的ICW1~ICW4控制关键字。8253时钟EFLAG=0中断，时钟工作。

lab1中完成了对哪些外设的访问？ (w2l2)
 ```
  + 采分点：说明了ucore OS访问的外设
  - 答案没有涉及如下3点；（0分）
  - 说明了时钟（1分）
  - 除第二点外，进一步说明了串口（2分）
  - 除上述两点外，进一步说明了并口，或说明了CGA，或说明了键盘（3分）
 ```
- [x]  

>  时钟、串口、并口(or键盘、CGA)。

lab1中的cprintf函数最终通过哪些外设完成了对字符串的输出？ (w2l2)
 ```
  + 采分点：说明了cprintf函数用到的3个外设
  - 答案没有涉及如下3点；（0分）
  - 说明了串口（1分）
  - 除第二点外，进一步说明了并口（2分）
  - 除上述两点外，进一步说明了CGA（3分）
 ```
- [x]  

>  串口、并口、CGA

---

## 小组思考题

---

lab1中printfmt函数用到了可变参，请参考写一个小的linux应用程序，完成实现定义和调用一个可变参数的函数。(spoc)
- [x]  
> 这个cpp程序完成了一个类似于snprintf的功能，不过只有整形、字符和字符串的格式化。
```
#include <string>
#include <iostream>
#include <stdarg.h>
#include <cstdlib>

using namespace std;

string formatStr(string fmt, ...)
{
    va_list ap;
    va_start(ap, fmt);

    string res = "";
    size_t len = fmt.size();
    bool formatFlag = false;
    for(size_t i = 0; i < len; i++)
    {
        //cout << "processing " << fmt[i] << endl;
        if(formatFlag)
        {
            switch(fmt[i])
            {
            case '%':
                {
                    res += '%';
                    break;
                }
            case 'd':
                {
                    int tmp = (int)va_arg(ap, int);
                    //cout << "1";
                    char buf[12];
                    itoa(tmp, buf, 10);
                    res += string(buf);
                    //cout << "2";
                    formatFlag = false;
                    //cout << "3";
                    break;
                }
            case 'c':
                {
                    char tmp = (char)va_arg(ap, int);
                    res += tmp;
                    formatFlag = false;
                    break;
                }
            case 's':
                {
                    char* tmp = (char*)va_arg(ap, char*);
                    res += tmp;
                    formatFlag = false;
                    break;
                }
            default:
                {
                    res += "%";
                    res += fmt[i];
                    formatFlag = false;
                    break;
                }
            }
        }
        else
        {
            if(fmt[i] == '%' && i != len-1)
            {
                formatFlag = true;
            }
            else
            {
                res += fmt[i];
            }
        }
        //cout << fmt[i] << " over" << endl;
    }
    //cout << res;
    return res;
}

int main()
{
    cout << formatStr("just a test:\nString:%s, %%f\nChar:%c\nint:%d+%d\n", "biu", 'f', 15, 16);
    return 0;
}
```



如果让你来一个阶段一个阶段地从零开始完整实现lab1（不是现在的填空考方式），你的实现步骤是什么？（比如先实现一个可显示字符串的bootloader（描述一下要实现的关键步骤和需要注意的事项），再实现一个可加载ELF格式文件的bootloader（再描述一下进一步要实现的关键步骤和需要注意的事项）...） (spoc)
- [x]  

> 先实现一个可显示字符串的bootloader，保证串口、并口、CGA的正常接入，执行工作有硬件设备初始化、分配RAM空间、设置堆栈、跳转到操作系统代码入口点。再实现一个可加载ELF格式文件的bootloader，注意对ELF格式的正确性要做出检查。执行工作有加载内核映像和设置内核映像执行参数。


如何能获取一个系统调用的调用次数信息？如何可以获取所有系统调用的调用次数信息？请简要说明可能的思路。(spoc)
- [x]  

> 在syscall函数中设置静态变量，或者建立一个全局变量，每次系统调用，对应信号的变量+1。

如何裁减lab1, 实现一个可显示字符串"THU LAB1"且依然能够正确加载ucore OS的bootloader？如果不能完成实现，请说明理由。
- [x]  

> 

对于ucore_lab中的labcodes/lab1，我们知道如果在qemu中执行，可能会出现各种稀奇古怪的问题，比如reboot，死机，黑屏等等。请通过qemu的分析功能来动态分析并回答lab1是如何执行并最终为什么会出现这种情况？
- [x]  

> 

对于ucore_lab中的labcodes/lab1,如果出现了reboot，死机，黑屏等现象，请思考设计有效的调试方法来分析常在现背后的原因。
- [x]  

> 

---

## 开放思考题

---

如何修改lab1, 实现在出现除零错误异常时显示一个字符串的异常服务例程的lab1？
- [x]  

> 


在lab1/bin目录下，通过`objcopy -O binary kernel kernel.bin`可以把elf格式的ucore kernel转变成体积更小巧的binary格式的ucore kernel。为此，需要如何修改lab1的bootloader, 能够实现正确加载binary格式的ucore OS？ (hard)
- [x]  

>

GRUB是一个通用的bootloader，被用于加载多种操作系统。如果放弃lab1的bootloader，采用GRUB来加载ucore OS，请问需要如何修改lab1, 能够实现此需求？ (hard)
- [x]  

>


如果没有中断，操作系统设计会有哪些问题或困难？在这种情况下，能否完成对外设驱动和对进程的切换等操作系统核心功能？
- [x]  

>  

---

## 各知识点的小练习

### 4.1 启动顺序
---
读入ucore内核的代码？

- [x]  

> 

跳转到ucore内核的代码？

- [x]  

> 

全局描述符表的初始化代码？

- [x]  

> 

GDT内容的设置格式？初始映射的基址和长度？特权级的设置位置？

- [x]  

> 

可执行文件格式elf的各个段的数据结构？

- [x]  

> 

如果ucore内核的elf是否要求连续存放？为什么？

- [x]  

> 
---

### 4.2 C函数调用的实现
---

函数调用的stackframe结构？函数调用的参数传递方法有哪几种？
- [x]  

> 

系统调用的stackframe结构？系统调用的参数传递方法有哪几种？

- [x]  

> 
---

### 4.3 GCC内联汇编
---

使用内联汇编的原因？

- [x]  

> 特权指令、性能优化

对ucore中的一段内联汇编进行完整的解释？

- [x]  

> 
---

### 4.4 x86中断处理过程
---

4.4 x86中断处理过程

中断描述符表IDT的结构？

- [x]  

> 

中断描述表到中断服务例程的地址计算过程？

- [x]  

> 

中断处理中硬件压栈内容？用户态中断和内核态中断的硬件压栈有什么不同？

- [x]  

> 

中断处理中硬件保存了哪些寄存器？

- [x]  

> 
---

### 4.5 练习一 ucore编译过程
---

gcc编译、ld链接和dd生成两个映像对应的makefile脚本行？

- [x]  

> 
---

### 4.6 练习二 qemu和gdb的使用
---

qemu的命令行参数含义解释？

- [x]  

> 

gdb命令格式？反汇编、运行、断点设置

- [x]  

> 
---

### 练习三 加载程序
---

A20的使能代码分析？

- [x]  

> 

生成主引导扇区的过程分析？

- [x]  

> 

保护模式的切换代码？

- [x]  

> 

如何识别elf格式？对应代码分析？

- [x]  

> 

跳转到elf的代码？

- [x]  

> 
函数调用栈获取？

- [x]  

> 
---

### 4.8 练习四和五 ucore内核映像加载和函数调用栈分析
---

如何识别elf格式？对应代码分析？

- [x]  

> 

跳转到elf的代码？

- [x]  

> 

函数调用栈获取？
- [x]  

> 
---


### 4.9 练习六 完善中断初始化和处理
---

各种设备的中断初始化？
- [x]  

> 
中断描述符表IDT的排列顺序？
- [x]  

> 中断号
CPU加电初始化后中断是使能的吗？为什么？

- [x]  

> 
中断服务例程的入口地址在什么地方设置的？

- [x]  

> 
alltrap的中断号是在哪写入到trapframe结构中的？

- [x]  

> 
trapframe结构？
- [x]  

> 
---
