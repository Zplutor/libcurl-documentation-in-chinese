## 声明

```
CURLcode curl_global_init(long flags);
```

## 概要

libcurl全局初始化。

## 详情

该函数建立起libcurl需要的程序环境。可以把它当做是库加载器的额外工作。

> 备注
>
> * 这里的意思是，可以在加载libcurl的时候顺便调用`curl_global_init`。

该函数必须在程序（指代所有共享同一个内存空间的代码）调用其它任何libcurl函数之前至少调用一次。它所建立的环境在程序的生命周期中是不变的，所以多次调用的效果跟单次调用相同。

`flags`选项是一个位域，它确切地告诉libcurl初始化哪些特性（在下文介绍）。可以通过或操作把想要的位设置到一起。在一般的使用场景下，你必须设置`CURL_GLOBAL_ALL`。不要使用任何其它的值，除非你熟悉这些值并且想要去控制libcurl的内部行为。

该函数不是线程安全的。当程序中任何其它线程（例如一个共享了相同内存的线程）正在运行的时候，你不能调用它。这不仅仅意味着没有其它线程使用libcurl。因为`curl_global_init`调用的其它库的函数同样是非线程安全的，它可能与使用了这些库的其它线程冲突。

请参考《libcurl》中对全局环境需求的说明来获取如何使用该函数的详细信息。

## 标记

### CURL_GLOBAL_ALL

尽可能初始化所有特性。这个标记设置除了`CURL_GLOBAL_ACK_EINTR`之外的所有位。

### CURL_GLOBAL_SSL

初始化SSL。

### CURL_GLOBAL_WIN32

初始化Win32的套接字库。

### CURL_GLOBAL_NOTHING

不初始化任何额外的特性。这个标记不设置任何位。

### CURL_GLOBAL_DEFAULT

一个合理的默认值。它会同时初始化SSL和Win32。现在，这个标记的作用等同于`CURL_GLOBAL_ALL`。

### CURL_GLOBAL_ACK_EINTR

当设置了这个标记，curl会在连接或者等待数据时应答EINTR状态。否则，curl会等待直到经过了完整的超时。（在7.30.0加入）

> 疑问
> 
> * EINTR是什么？

## 返回值

如果该函数返回非0，说明发生了某些错误，你不能再使用其它curl函数。