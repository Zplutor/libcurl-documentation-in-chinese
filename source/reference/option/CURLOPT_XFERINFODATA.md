## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_XFERINFODATA, void *pointer);
```

## 概要

传递给进度回调的自定义指针。

## 详情

传入一个指针，该指针不会被libcurl使用，而是作为通过`CURLOPT_XFERINFOFUNCTION`设置的进度回调的第一个参数传入。

这是`CURLOPT_PROGRESSDATA`的一个别名。

## 默认值

该选项的默认值是NULL。

## 适用协议

所有协议。

## 可用性

在7.32.0加入。

## 返回值

返回`CURLE_OK`。