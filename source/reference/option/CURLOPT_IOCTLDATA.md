## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_IOCTLDATA, void *pointer);
```

## 概要

传递给I/O回调的自定义指针。

## 详情

传入的`pointer`不会被libcurl使用，并且是作为通过`CURLOPT_IOCTLFUNCTION`设置的ioctl回调的第三个参数。

## 默认值

默认情况下，该选项的值是NULL。

## 适用协议

用在HTTP。

## 可用性

在7.12.3加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。