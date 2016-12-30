## 声明

```
CURL *curl_easy_init();
```

## 概要

开始一个licurl easy会话。

## 详情

该函数必须是第一个调用的函数，它会返回一个CURL easy句柄，你必须把它作为其它easy接口函数的输入。在操作完成后，必须有一个对应的`curl_easy_cleanup`调用。

如果你还没有调用`curl_global_init`，`curl_easy_init`会自动做这个事情。这在多线程的情况下可能是致命的，因为`curl_global_init`不是线程安全的，这可能会导致资源问题，因为不存在对应的清理过程。

强烈建议你不要允许这个自动行为，而是自己正确地调用`curl_global_init`。参考《libcurl(3)》中对全局环境需求的描述来获取如何使用这个函数的详细信息。

## 返回值

如果该函数返回`NULL`，说明出现了某些错误，你不能再使用其它curl函数。