## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_STDERR, FILE *stream);
```

## 概要

重定向stderr到其它流。

## 详情

传入一个`FILE*`作为参数。告诉libcurl在显示进度计量以及显示`CURLOPT_VERBOSE`数据的时候使用这个流而不是stderr。

## 默认值

stderr。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。