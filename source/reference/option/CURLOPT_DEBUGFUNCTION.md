## 声明

```
typedef enum {
  CURLINFO_TEXT = 0,
  CURLINFO_HEADER_IN,    /* 1 */
  CURLINFO_HEADER_OUT,   /* 2 */
  CURLINFO_DATA_IN,      /* 3 */
  CURLINFO_DATA_OUT,     /* 4 */
  CURLINFO_SSL_DATA_IN,  /* 5 */
  CURLINFO_SSL_DATA_OUT, /* 6 */
  CURLINFO_END
} curl_infotype;
 
int debug_callback(CURL *handle,
                   curl_infotype type,
                   char *data,
                   size_t size,
                   void *userptr);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_DEBUGFUNCTION,
                          debug_callback);
```

## 概要

调试回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

`CURLOPT_DEBUGFUNCTION`会替换掉当`CURLOPT_VERBOSE`生效时使用的标准调试函数。这个回调接收通过`type`参数指定的调试信息。这个函数必须返回0。传递给这个函数的通过`char*`指向的`data`不会以0结束，但会跟`size`参数指定的大小准确地一致。

`userptr`参数是通过`CURLOPT_DEBUGDATA`设置的指针。

可用的`curl_infotype`值：

### CURLINFO_TEXT

数据是信息性的文本。

### CURLINFO_HEADER_IN

数据是从另一端接收到的头部（或者类似头部的数据）。

### CURLINFO_HEADER_OUT

数据是发送给另一端的头部（或者类似头部的数据）。

### CURLINFO_DATA_IN

数据是从另一端接收到的协议数据。

### CURLINFO_DATA_OUT

数据是发送给另一端的协议数据。

### CURLINFO_SSL_DATA_OUT

数据是发送给另一端的SSL/TLS（二进制）数据。

### CURLINFO_SSL_DATA_IN

数据是从另一端接收到的SSL/TLS（二进制）数据。

## 默认值

无。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。