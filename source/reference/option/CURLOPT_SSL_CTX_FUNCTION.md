## 声明

```
CURLcode ssl_ctx_callback(CURL *curl, void *ssl_ctx, void *userptr);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SSL_CTX_FUNCTION,
                          ssl_ctx_callback);
```

## 概要

OpenSSL或wolfSSL/CyaSSL的SSL上下文回调。

## 详情

这个选项仅在由OpenSSL或者wolfSSL/CyaSSL驱动的libcurl上生效。如果libcurl与其它SSL库构建的话是没有这个功能的。

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

这个回调函数会在以下时机被libcurl调用：在一个SSL连接初始化之前，并且在处理了所有其它SSL相关的选项之后，以此让一个应用程序有一个最后的机会去修改SSL初始化的行为。`ssl_ctx`参数实际上是一个指向SSL库的`SSL_CTX`。如果从回调中返回了一个错误，libcurl不会尝试去建立连接，并且正在执行的操作会返回回调的错误码。使用`CURLOPT_SSL_CTX_DATA`选项设置`userptr`参数。

这个函数会在所有与服务器的新连接上被调用，在SSL协商的过程中。`SSL_CTX`指针每次都会是新的。

为了正确地使用这个选项，对你的SSL库有一定的了解是必需的。例如，你可以使用这个函数去调用跟库相关的回调来添加对证书的额外验证代码，甚至去修改一个HTTPS请求的实际URI（在lib509测试用例中使用的例子）。参考示例一节来获知如何替换键，证书和信任文件设置。

## 默认值

无。

## 适用协议

所有基于TLS的协议：HTTPS，FTPS，IMAPS，POP3，SMTPS等。

## 可用性

在7.11.0针对OpenSSL加入。在7.42.0针对wolfSSL/CyaSSL加入。其它的SSL后端未支持。

## 返回值

如果该选项被支持，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。