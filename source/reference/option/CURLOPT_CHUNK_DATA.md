## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CHUNK_DATA, void *pointer);
```

## 概要

传给FTP分块回调的自定义指针。

## 详情

传入一个指针`pointer`，这个指针不会被libcurl使用，而是作为`ptr`参数传递给`CURL_CHUNK_BGN_FUNCTION`和`CURL_CHUNK_END_FUNCTION`。

## 默认值

NULL。

## 适用协议

FTP。

## 可用性

在7.21.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。