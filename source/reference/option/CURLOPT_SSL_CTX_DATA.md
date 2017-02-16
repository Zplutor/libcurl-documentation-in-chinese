## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SSL_CTX_DATA, void *pointer);
```

## 概要

传递给ssl_ctx回调的自定义指针。

## 详情

传递给通过`CURLOPT_SSL_CTX_FUNCTION`选项设置的SSL上下文回调的数据指针，这是你会在回调的第三个参数中获得的指针。

## 默认值

无。

## 适用协议

所有基于TLS的协议：HTTPS，FTPS，IMAPS，POP3，SMTPS等。

## 可用性

在7.11.0针对OpenSSL加入。在7.42.0针对wolfSSL/CyaSSL加入。其它SSL后端未支持。

## 返回值

如果该选项被支持，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。