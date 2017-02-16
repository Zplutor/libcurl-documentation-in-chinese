## 声明

```
CURLcode conv_callback(char *ptr, size_t length);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_CONV_TO_NETWORK_FUNCTION,
                          conv_callback);
```

## 概要

把数据从本机编码转换成网络编码。

## 概要

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

该选项应用于非ASCII平台。如果提供了这个选项，`curl_version_info`会返回`CURL_VERSION_CONV`特性位。

被转换的数据位于由`ptr`参数指向的的缓冲区中。转换的数据大小由`length`参数指定。转换后的数据会覆盖由`ptr`参数指向的缓冲区中的输入数据。当转换成功的时候，必须返回`CURLE_OK`。当发生错误时，应该返回一个在`curl.h`中定义的`CURLcode`返回值，例如`CURLE_CONV_FAILED`。

`CURLOPT_CONV_TO_NETWORK_FUNCTION`从本机编码转换成网络编码。它在通过网络发送命令或者ASCII数据时被使用。

如果你设置回调指针为NULL，或者根本不设置它，那么内置的libcurl iconv函数会被使用。如果在构建libcurl的时候没有定义`HAVE_ICONV`，而且没有设置回调，转换的时候会返回`CURLE_CONV_REQD`错误码。

如果定义了`HAVE_ICONV`，那么一定要同时定义`CURL_ICONV_CODESET_OF_HOST`。例如：

```
#define CURL_ICONV_CODESET_OF_HOST "IBM-1047"
```

libcurl的iconv代码会像下面那样设置默认的网络和UTF8编码集的名称：

```
 #define CURL_ICONV_CODESET_OF_NETWORK "ISO8859-1"
 #define CURL_ICONV_CODESET_FOR_UTF8 "UTF-8"
```

如果它们在你的系统上不一样，你需要重写这些定义。

> 疑问
> * 如何确定本机编码和网络编码？是否在构建libcurl的时候通过`CURL_ICONV_CODESET_OF_HOST`和`CURL_ICONV_CODESET_OF_NETWORK`来指定？

## 默认值

无。

## 适用协议

FTP，SMTP，IMAP，POP3。

## 可用性

只在构建libcurl时定义了`CURL_DOES_CONVERSIONS`的情况下可用。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。