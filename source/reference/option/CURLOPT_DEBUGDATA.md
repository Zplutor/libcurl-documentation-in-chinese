## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_DEBUGDATA, void *pointer);
```

## 概要

给调试回调的自定义指针。

## 详情

对`pointer`传入一个任何你想要的指针，该指针会在最后一个`void*`参数中传入给你的`CURLOPT_DEBUGFUNCTION`。这个指针不会被libcurl使用，它只会被传递给回调。

## 默认值

无。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。