## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CLOSESOCKETDATA, void *pointer);
```

## 概要

传递给套接字关闭回调的指针。

## 详情

传入一个指针，该指针不会被libcurl使用，并且会在通过`CURLOPT_CLOSESOCKETFUNCTION`设置的关闭套接字回调中作为第一个参数。

## 默认值

该选项的默认值是NULL。

## 适用协议

所有协议。

## 可用性

在7.21.7加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。