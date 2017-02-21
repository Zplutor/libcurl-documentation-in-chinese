## 声明

```
long chunk_end_callback(void *ptr);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CHUNK_END_FUNCTION,
                          chunk_end_callback);
```

## 概要

在使用FTP通配符匹配的传输后的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

只要流的一部分传输完（或者被跳过），libcurl就会调用这个函数。

如果没有任何错误，返回`CURL_CHUNK_END_FUNC_OK`；如果有错误的话返回`CURL_CHUNK_END_FUNC_FAIL`让libcurl停止传输。

## 默认值

NULL。

## 适用协议

FTP。

## 可用性

在7.21.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。