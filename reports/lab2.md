# lab2

（刚知道不用写报告，气死我了）

## 实现功能

### fix sys_get_time && sys_task_info

通过用户态页表将用户态地址转换为物理地址。

将传入的 \*TimeVal 地址转换为内核态可用的指针，然后填充结构体。

sys_task_info 同理。

### sys_mmap

首先对输入参数做检查，非法参数返回 -1 。

然后检查将要 mmap 的地址对应的 vpn 是否已经 map ，如果已经 map 了，返回 -1 ，否则将这一页 mmap 到一个匿名 frame 。

传入的 port 需要转换成 MapPermission ，然后设置 U 位。

### sys_munmap

检查将要 unmap 的地址是否都是已经被 map ，如果 unmap 一段没有被 map 的地址会返回 -1 。

检查 vpn 是否 map 的方式，是通过 page table 获取 pte ，如果 ptr 存在且有效，就认为已经 map。所以 unmap 也是调用 page table 的 unmap 。

尝试操作 data_frames ，但是似乎有 bug ，无法正常调用 remove 函数，所以就没用。

## 问答题

### 1

![](https://learningos.github.io/rust-based-os-comp2022/_images/sv39-pte.png)

上图为 SV39 分页模式下的页表项，其中 [53:10] 这 44 位是物理页号，最低的 8 位 [7:0] 则是标志位，它们的含义如下：

仅当 V(Valid) 位为 1 时，页表项才是合法的；

R/W/X 分别控制索引到这个页表项的对应虚拟页面是否允许读/写/取指；

U 控制索引到这个页表项的对应虚拟页面是否在 CPU 处于 U 特权级的情况下是否被允许访问；

G 是 Global；

A(Accessed) 记录自从页表项上的这一位被清零之后，页表项的对应虚拟页面是否被访问过；

D(Dirty) 则记录自从页表项上的这一位被清零之后，页表项的对应虚拟页表是否被修改过。

### 2



## 其它

小语法错误。

```diff
- 一定要注意 mmap 是的页表项，注意 riscv 页表项的格式与 port 的区别。
+ 一定要注意 mmap 的是页表项，注意 riscv 页表项的格式与 port 的区别。
```
