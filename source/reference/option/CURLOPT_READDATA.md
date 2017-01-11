## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_READDATA, void *pointer);
```

## 概要

传递给读回调的自定义指针。

## 详情

设置传递给文件读取函数的数据`pointer`指针。如果你使用了`CURLOPT_READFUNCTION`选项，这是你会在回调输入的第四个参数获得的指针。

如果你没有指定一个读取回调，而是依赖默认的内部读取函数，这个数据必须是一个有效且可读的`FILE*`（转换成`void*`）。

如果你正在以win32 DLL的方式使用libcurl，并且设置了这个选项，你必须使用`CURLOPT_READFUNCTION`。

## 默认值

默认情况下，这个选项是一个指向stdin的FILE*。

## 适用协议

该选项适用于所有发送数据的协议。

## 可用性

该选项曾经的旧名字是`CURLOPT_INFILE`，`CURLOPT_READDATA`这个名字在7.9.7引入。

## 返回值

该选项会返回`CURLE_OK`。