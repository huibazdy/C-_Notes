# Binder

## 一、全景视图



> ***基于内存映射***

目的是减少数据拷贝次数。



> ***四大组件***

1. **Client**
2. **Server**
3. **ServiceManager**
4. **Binder Driver**

前三者在用户空间实现，驱动实现于内核空间。其中 3 和 4 都是 Android 原生实现，作为开发者只需要按照规定在用户空间实现自己的 Client 与 Server 即可。

ServiceManager 是一个守护进程，同时也是一个特殊的 Server 。

驱动提供设备文件`/dev/binder`与用户空间交互，交互的接口一般是：`open`或`ioctl`。



## 二、源码分析

### ServiceManager

> 需要弄清楚的几个问题：
>
> 1. 怎样称为守护进程的？
> 2. 如何告知 Binder 驱动它是 Binder 机制的管理者？
> 3. Client 和 Server 分别是如何获取 ServiceManager 的接口的（defaultServiceManager接口如何实现）？
> 4. Server 如何启动服务？
> 5. ServiceManager 在 Server 启动过程中如何为它提供服务（IServiceManager::addService 如何实现）？
> 6. ServiceManager 如何为 Client 提供服务（IServiceManager::getService 如何实现）？

### 

和 servicemanager 相关的代码路径：`frameworks/native/cmds/servicemanager/`