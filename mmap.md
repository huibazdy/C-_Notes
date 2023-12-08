# mmap

用户空间的系统调用



```c
#include <unistd.h>
#include <sys/mman.h>

void* mmap(void* start, size_t length, int prot, int flags, int fd, off_t offset);
```



【**参数说明**】

* 请求内核新创建虚拟内存区域的起始地址
* 被映射对象文件的被映射片区大小
* 新的虚拟内存区域的访问权限，包含多个权限位
* 描述被映射对象的权限位
* 被映射对象文件的文件描述符
* 被映射片区的偏移量



# munmap

用于删除虚拟内存区域



```c
#include <unistd.h>
#include <sys/mman.h>

int munmap(void* start, size_t length);
```



* 从待删除虚拟内存区域的起始地址开始
* 删除长度是 length 字节



