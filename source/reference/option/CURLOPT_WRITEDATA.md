## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_WRITEDATA, void *pointer);
```

## 概要

设置传递给写回调的自定义指针。

## 详情

设置一个传递给写回调的数据指针`pointer`。如果你使用了`CURLOPT_WRITEFUNCTION`选项，这个指针就是你在那个回调获得的第四个参数。如果你没有使用一个写回调，你必须使得`pointer`是一个`FILE*`（转换成`void*`），因为libcurl会在写数据的时候把它传递给`fwrite`。

内部的`CURLOPT_WRITEFUNCTION`会把数据写到这个选项指定的FILE*，或者会写到stdout，如果这个选项还没有设置的话。

如果你正在以win32 DLL的方式使用libcurl，而且你设置了该选项，那么你必须使用`CURLOPT_WRITEFUNCTION`，否则你会遇到程序崩溃。

> 疑问
>
> * 为什么会崩溃？

## 默认值

默认情况下，这是一个指向stdout的FILE*。

## 适用协议

所有协议。

## 可用性

在所有libcurl版本上都可用。这个选项原来以`CURLOPT_FILE`存在，`CURLOPT_WRITEDATA`这个名称是在7.9.7引入的。

## 返回值

该选项会返回`CURLE_OK`。