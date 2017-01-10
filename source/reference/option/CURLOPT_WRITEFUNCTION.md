## 声明

```
size_t write_callback(char *ptr, size_t size, size_t nmemb, void *userdata);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_WRITEFUNCTION, write_callback);
```

## 概要

设置写接收数据的回调。

## 详情

传入一个你的回调函数的指针，该函数应该符合上面显示的原型。

一旦接收到的数据需要保存，libcurl就会调用这个回调函数。`ptr`指向已接收的数据，并且数据的大小是`size`乘上`nmemb`。

libcurl在每次调用该回调函数的时候都会传入尽可能多的数据，但你一定不能做任何假设。它可能是一个字节，也可能是上千字节。可以传递给写回调的最大正文数据数量在`curl.h`头文件中定义：`CURL_MAX_WRITE_SIZE`（默认值通常是16K）。如果`CURLOPT_HEADER`被开启，它会使得头部数据传递给写回调，你可以得到至多`CURL_MAX_HTTP_HEADER`字节的头部数据。这通常意味着是100K。

如果传输的文件为空， 这个函数可能会以0字节数据被调用。

传递给这个函数的数据不是以0结束的！

使用`CURLOPT_WRITEDATA`选项来设置`userdata`参数。

你的回调应该返回实际处理的字节数量。如果这个数量与传给你的回调函数的数量不一致，它会标识一个错误状态给libcurl。这会导致传输被终止，并且正在使用的libcurl函数会返回`CURLE_WRITE_ERROR`。

如果你的回调函数返回`CURL_WRITEFUNC_PAUSE`，它会导致这个传输暂停。参考`curl_easy_pause`获取更多详情。

把这个选项设置成NULL会使用内部默认的函数，而不是使用你的回调。内部默认的函数会把数据写到`CURLOPT_WRITEDATA`	提供的FILE*中。

## 默认值

默认情况下libcurl会使用`fwrite`作为回调。

## 适用协议

所有协议。

## 可用性

对`CURL_WRITEFUNC_PAUSE`返回值的支持在7.18.0版本中添加。

## 返回值

该选项会返回`CURLE_OK`。