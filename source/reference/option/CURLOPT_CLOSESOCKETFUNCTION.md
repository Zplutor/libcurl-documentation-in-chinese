## 声明

```
int closesocket_callback(void *clientp, curl_socket_t item);

CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CLOSESOCKETFUNCTION, closesocket_callback);
```

## 概要

替换关闭套接字函数的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该匹配上面显示的原型。

当套接字被关闭的时候（不是其它类型的文件描述符），libcurl会调用这个回调函数，而不是调用`close`或`closesocket`。这基本上是`CURLOPT_OPENSOCKETFUNCTION`选项的反过程。返回0来标识成功，如果有错误的话返回1。

`clientp`指针通过`CURLOPT_CLOSESOCKETDATA`来设置。`item`是libcurl想要关闭的套接字。

## 默认值

默认情况下，libcurl使用标准的套接字关闭函数。

## 适用协议

所有协议。

## 可用性

在7.21.7加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。