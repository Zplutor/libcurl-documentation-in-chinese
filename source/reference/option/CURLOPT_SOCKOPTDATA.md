## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SOCKOPTDATA, void *pointer);
```

## 概要

传递给sockopt回调的自定义指针。

## 详情

传入的`pointer`不会被libcurl使用，它作为第一个参数传递给通过`CURLOPT_SOCKOPTFUNCTION`设置的sockopt回调。

## 默认值

这个选项的默认值是NULL。

## 适用协议

所有协议。

## 可用性

在7.16.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。