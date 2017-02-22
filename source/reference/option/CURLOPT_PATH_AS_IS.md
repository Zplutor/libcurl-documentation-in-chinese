## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_PATH_AS_IS, long leaveit);
```

## 概要

不要处理点点序列。

## 详情

通过设置`long`类型的`leaveit`参数为1，显式地告诉libcurl在把路径传递给服务器之前不要修改它。

这个选项告诉libcurl不要压缩可能存在于URL路径部分的`/../`和`/./`序列。按照RFC 3986的5.2.4节，这些序列应该要被移除。

一些已知的服务器实现（错误地）要求点点序列保留在路径中，因而有些客户端为了测试出服务器的实现而想要传递这些路径。

默认情况下libcurl会在使用路径之前合并这些序列。

## 默认值

0。

## 适用协议

所有协议。

## 可用性

在7.42.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。