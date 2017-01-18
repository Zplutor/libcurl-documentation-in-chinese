## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_OPENSOCKETDATA, void *pointer);
```

## 概要

传递给打开套接字回调的自定义指针。

## 详情

传入一个指针，该指针不会被libcurl使用，它会在通过`CURLOPT_OPENSOCKETFUNCTION`设置的打开套接字回调中作为第一个参数。

## 默认值

这个选项的默认值是NULL。

## 适用协议

所有协议。

## 可用性

在7.17.1加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。