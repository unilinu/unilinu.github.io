## 《Linux/Unix系统编程手册》读书笔记

### 参考文献

- [读书笔记](http://blog.xiayf.cn/gitbook/tech-note/basic/system/tlpi.html)


### 第2章：基本概念

一般情况下，Linux内核可执行文件采用`/boot/vmlinuz`或与之类似的路径名。早期的UNIX实现称其内核为UNIX。在后续实现了虚拟内存机制的UNIX系统中，其内核名称变更为vmunix。对Linux来说，文件名称中的系统名需要调整，而以“z”替换“linux”末尾的“x”，意在表明内核是经过压缩的可执行文件。

在文件系统内，会对文件类型进行标记，以表明其种类。其中一种用来表示普通数据文件，常称之为“普通文件”或“纯文本文件”。其他文件类型包括设备、管道、套接字、目录以及符号链接。

目录是一种特殊类型的文件，内容采用表格形式，数据项包括文件名以及对相应文件的引用。这一“文件名+引用”的组合被称为链接。每个文件都可以有多条链接，因而也可以有多个名称，在相同或不同的目录中出现。

进程的当前工作目录继承自其父进程。

每个文件都有一个与之相关的用户ID和组ID，分别定义文件的属主和属组。系统根据文件的所有权来判定用户对文件的访问权限。

对于目录而言，权限设置的意义与文件不同： **读权限允许列出目录内容（即该目录下的文件名），写权限允许对目录内容进行更改（比如，添加、修改或删除文件名），执行（有时也称为搜索）权限允许对目录中的文件进行访问（但需受文件自身访问的权限的约束）** 。

进程的内存布局 - 逻辑上将一个进程划分为以下几部分（也称为段）：

- 文本：程序的指令
- 数据：程序使用的静态变量
- 堆：程序可以从该区域动态分配额外内存
- 栈：随函数调用、返回而增减的一片内存，用于为局部变量和函数调用链接信息分配存储空间

内核通过对父进程的复制来创建子进程。子进程从父进程处继承数据段、栈段以及堆段的副本后，可以修改这些内容，不会影响父进程的“原版”内容。（在内存中被标记为只读的程序文本段则由父、子进程共享。）

然后，子进程要么去执行与父进程共享代码段中的另一组不同函数，或者，更为常见的情况是使用系统调用execve()去加载并执行一个全新程序。execve()会销毁现有的文本段、数据段、栈段及堆段，并根据新程序的代码，创建新段来替换它们。

可使用以下两种方式之一来终止一个进程：其一，进程可使用 `_exit()` 系统调用（或相关的 `exit()` 库函数），请求退出；其二，向进程传递信号，将其“杀死”。无论以何种方式退出，进程都会生成“终止状态”，一个非负小整数，可供父进程的 `wait()` 系统调用检测。

**进程的用户和组标识符（凭证）**

每个进程都有一组与之相关的用户ID（UID）和组ID（GID）：

- 真实用户ID和组ID：用来标识进程所属的用户和组。新进程从其父进程处继承这些ID。登录shell则会从系统密码文件的相应字段中获取其真实用户ID和组ID。
- 有效用户ID和组ID：进程在访问受保护资源（比如，文件和进程间通信对象）时，会使用这两个ID（并结合下述的补充组ID）来确定访问权限。一般情况下，进程的有效ID和相应的真实ID值相同。改变进程的有效ID实为一种机制，可使进程具有其他用户或组的权限（命令 `sudo` 的作用？）
- 补充组ID：用来标识进程所属的额外组。新进程从其父进程处继承补充组ID。登录shell则从系统组文件中获取其补充组ID。

每个进程都会消耗诸如打开文件、内存以及CPU时间之类的资源。使用系统调用 `setrlimit()` ，进程可为自己消耗的各类资源限定一个上限。此类资源限制的每一项均有两个相关值：软限制（soft limit）限制了进程可以消耗的资源总量，硬限制（hard limit）限制软限制的调整上限。非特权进程在针对特定资源调整软限制值时，可将其设置为0到相应硬限制值之间的任意值，但硬限制值则只能调低，不能调高。

由 `fork()` 创建的新进程，会继承其父进程对资源限制的设置。

使用ulimit命令可调整shell的资源限制。shell为执行命令所创建的子进程会继承上述资源设置。

进程间通信（IPC）机制：

- 信号（signal）：用来表示事件的发生
- 管道和FIFO：用于在进程间传递数据
- 套接字：供同一台主机或是联网的不同主机上所运行的进程间传递数据
- 文件锁定：为防止其他进程读取或更新文件内容，允许某进程对文件的部分区域加以锁定
- 消息队列：用于在进程间交换消息（数据包）
- 信号量（semaphore）：用来同步进程动作
- 共享内存：允许两个及两个以上进程共享一块内存。当某进程改变了共享内存的内容时，其他所有进程会立即了解到这一变化

### 第3章：系统编程概念

系统调用处理流程：

1. 应用程序通过调用C语言函数库中的外壳（wrapper）函数，发起系统调用;

2. 对系统调用中断处理程序来说，外壳函数必须保证所有的系统调用参数可用。通过堆栈，这些参数传入外壳函数，但内核却希望将这些参数置入特定寄存器。因此，外壳函数会将上述参数复制到寄存器;

3. 由于所有系统调用进入内核的方式相同，内核需要设法区分每个系统调用。为此，外壳函数会将系统调用编号复制到一个特殊的CPU寄存器（%eax）中;

4. 外壳函数执行一条中断机器指令（int 0x80），引发处理器从用户态切换到内核态，并执行系统中断0x80（十进制数128）的中断矢量所指向的代码。

5. 为响应中断0x80,内核会调用

    

   ```
   system_call()
   ```

    

   例程（位于汇编文件

    

   ```
   arch/i386/entry.S
   ```

    

   中）来处理这次中断，具体如下：

   - 在内核栈中保存寄存器值
   - 审核系统调用编号的有效性
   - 以系统调用编号对存放所有调用服务例程的列表（内核变量 `sys_call_table` ）进行索引，发现并调用相应的系统调用服务例程。如果系统调用服务例程带有参数，那么将首先检查参数的有效性。随后，该服务例程会执行必要的任务，这可能涉及对特定参数中指定地址处的值进行修改，以及在用户内存和内核内存间传递数据。最后，该服务例程会将结果状态返回给 `system_call()` 例程。
   - 从内核栈中恢复各寄存器值，并将系统调用返回值置于栈中。
   - 返回至外壳函数，同时将处理器切换回用户态。

6. 若系统调用服务例程的返回值表明调用有误，外壳函数会使用该值来设置全局变量errno。然后，外壳函数会返回到调用程序，并同时返回一个整型值，以表明系统调用是否成功。

### 第5章：深入探究文件I/O

所有系统调用都是以原子操作方式执行的。之所以这么说，是指内核保证了某系统调用中的所有步骤会作为独立操作而一次性加以执行，其间不会为其他进程或线程所中断。

竞争状态（race conditions）或竞争冒险，是这样一种情形：**操作共享资源的两个进程（或线程），其结果取决于一个无法预期的顺序，即这些进程/线程获得CPU使用权的先后相对顺序**。

内核为文件操作维护着三个数据结构：

- 进程级的文件描述符表
- 系统级的打开文件表
- 文件系统的i-node表（这里指的是对应硬盘上i-node记录在内存中的副本）

### 第7章：内存分配

通常情况下，当增大已分配内存时，`realloc()` 会试图去合并在空闲列表中紧随其后且大小满足要求的内存块。若原内存块位于堆的顶部，那么 `realloc()` 将对堆空间进行扩展。如果这块内存位于堆的中部，且紧邻其后的空闲内存空间大小不足， `realloc()` 会分配一块新内存，并将原有数据复制到新内存块中。最后这种情况最为常见，还会占用大量CPU资源。一般情况下，应尽量避免调用 `realloc()` 。

### 第9章：进程凭证

实际用户ID和实际组ID确定了进程所属的用户和组。

在大多数UNIX实现中，当进程尝试执行各种操作（即系统调用）时，将结合有效用户ID、有效组ID，连同辅助组ID一起来确定授予进程的权限 。

有效用户ID为0（root的用户ID）的进程拥有超级用户的所有权限。这样的进程又称为特权级进程（privileged process）。而某些系统调用只能由特权级进程执行。

通常，有效用户ID及组ID与其相应的实际ID相等，但有两种方式能够致使二者不同。其一是使用9.7节中所讨论的系统调用，其二是执行set-user-ID和set-group-ID程序。

实际ID定义了进程所属。在大多数的UNIX实现中，进程对诸如文件之类资源的访问，其许可权限由有效ID决定。然而，Linux会使用文件系统ID来决定对文件的访问权限，而将有效ID用于检查其他权限。（因为文件系统ID一般等同于相应的有效ID，所以Linux对文件权限的检查方式与其他UNIX实现相同。）

### 第10章：时间

![time-system-call](http://blog.xiayf.cn/gitbook/tech-note/basic/image/time-system-calls.png)

SUSv3规定，调用`ctime()`、`gmtime()`、`localTime()`或`asctime()`中的任一函数，都可能会覆盖由其他函数返回，且经静态分配的数据结构。换言之，这些函数可以共享返回的字符数据和tm结构体。

系统的本地时区由时区文件`/etc/localtime`定义，通常链接到`/usr/share/zoneinfo`下的一个文件。

为运行中的程序指定一个时区，需要将TZ环境变量设置为由一冒号（:）和时区名称组成的字符串，其中时区名称定义于`/usr/share/zoneinfo`中。设置时区会自动影响到函数`ctime()`、`localtime()`、`mktime()`和`strftime()`。为了获取当前的时区设置，上述函数都会调用`tzset(3)`。函数`tzset()`会首先检查环境变量TZ。如果尚未设置该变量，那么就采用`/etc/localtime`中定义的默认时区来初始化时区。如果TZ环境变量的值为空，或无法与时区文件名相匹配，那么就使用UTC。




