## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_HEADERDATA, void *pointer);
```

## 概要

传递给头部回调的指针。

## 详情

传入一个指针，该指针会用来写接收数据的头部部分。

如果使用了`CURLOPT_WRITEFUNCTION`或者`CURLOPT_HEADERFUNCTION`，`pointer`会传递给各自的回调。

如果那些选项都没有设置，`pointer`必须是一个有效的`FILE*`，它会被`fwrite`用来写头部。

## 默认值

NULL。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回CURLE_OK。