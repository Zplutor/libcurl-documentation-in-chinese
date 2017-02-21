## 声明

```
long chunk_bgn_callback(const void *transfer_info, void *ptr,
                        int remains);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CHUNK_BGN_FUNCTION,
                          chunk_bgn_callback);
```

## 概要

在使用FTP通配符匹配的传输前的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

在流的一部分将要开始传输之前，libcurl会调用这个回调函数（如果传输支持分块）。

`transfer_info`指针指向一个`curl_fileinfo`结构，该结构包含了关于将要传输的文件的详细信息。

这个选项只在使用了`CURLOPT_WILDCARDMATCH`选项的情况下才有意义。

`transfer_info`参数指向的目标是一个“特性依赖”的结构。对于FTP通配符下载，指向的目标是`curl_fileinfo`结构（参考`curl/curl.h`）。`ptr`参数是通过`CURLOPT_CHUNK_DATA`指定的指针。`remains`参数包含了当前传输中剩下的的分块数量。如果这个特性不可用，参数的值都是0。

如果没有任何错误，返回`CURL_CHUNK_BGN_FUNC_OK`；如果你想跳过具体的分块，返回`CURL_CHUNK_BGN_FUNC_SKIP`；如果发生了错误，返回`CURL_CHUNK_BGN_FUNC_FAIL`告诉libcurl停止传输。

## 默认值

NULL。

## 适用协议

FTP。

## 可用性

在7.21.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。
