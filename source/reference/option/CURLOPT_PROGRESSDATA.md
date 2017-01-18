## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_PROGRESSDATA, void *pointer);
```

## 概要

传递给进度回调的自定义指针。

## 详情

传入一个指针，该指针不会被libcurl使用，并且会在通过`CURLOPT_PROGRESSFUNCTION`设置的进度回调中作为第一个参数。

## 默认值

该选项的默认值为NULL。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。