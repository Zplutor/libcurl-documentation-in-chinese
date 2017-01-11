## 声明

```
typedef enum {
  CURLIOE_OK,            /* I/O operation successful */
  CURLIOE_UNKNOWNCMD,    /* command was unknown to callback */
  CURLIOE_FAILRESTART,   /* failed to restart the read */
  CURLIOE_LAST           /* never use */
} curlioerr;
 
typedef enum  {
  CURLIOCMD_NOP,         /* no operation */
  CURLIOCMD_RESTARTREAD, /* restart the read stream from start */
  CURLIOCMD_LAST         /* never use */
} curliocmd;
 
curlioerr ioctl_callback(CURL *handle, int cmd, void *clientp);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_IOCTLFUNCTION, ioctl_callback);
```

## 概要

I/O操作的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该匹配上面显示的原型。

当libcurl不能自己执行一些I/O相关的特殊操作时，它会调用这个回调函数。目前，回溯读取数据流是它唯一会请求的行为。当以多通道的身份验证方式执行一个HTTP PUT或者POST时，回溯读取数据流可能是必需的。

如果输入的`cmd`不是`CURLIOCMD_RESTARTREAD`，回调必须返回`CURLIOE_UNKNOWNCMD`。

传给回调的`clientp`参数通过`CURLOPT_IOCTLDATA`选项来设置。

这个选项是被废弃的！不要使用它。应使用`CURLOPT_SEEKFUNCTION`来提供重定位！如果设置了`CURLOPT_SEEKFUNCTION`，这个选项在重定位的时候会被忽略。

## 默认值

默认情况下，这个选项被设置成NULL。即不使用。

## 适用协议

用在HTTP。

## 可用性

在7.12.3加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。