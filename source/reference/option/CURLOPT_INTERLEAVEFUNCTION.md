## 声明

```
size_t interleave_callback(void *ptr, size_t size, size_t nmemb,
                           void *userdata);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_INTERLEAVEFUNCTION,
                          interleave_callback);
```

## 概要

RTSP交叉数据的回调函数。

## 详情

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

只要libcurl接收到交叉的RTP数据，该回调函数就会被调用。对于每一个`$`块，这个函数都会被调用一次，因此它包含精确的一个上层协议单元（例如一个RTP包）。对于每一次调用，libcurl都会写入交叉头部以及包含的数据。第一个字节总是一个ASCII的美元符号。在美元符号后面的是一个字节的渠道标识符，然后是以网络字节序表示的两个字节的整型长度。参考RFC 2326的10.12节来获取更多关于RTP交叉行为的更多信息。如果该选项没有设置，或者设置成NULL，libcurl会使用默认的写函数。

交叉RTP对客户端应用程序产生一些挑战。由于流式数据共享RTSP控制连接，以及时的方式服务RTP是很重要的。如果RTP数据不能被很快地处理，后续的响应处理可能会无故地延迟，并且连接可能会关闭。当没有请求要处理的时候，应用程序可以使用`CURL_RTSPREQ_RECEIVE`来服务RTP数据。如果应用程序生成了一个请求，（例如`CURL_RTSPREQ_PAUSE`）那么响应处理器会在标记请求已完成之前处理完任何待处理的RTP数据。

## 默认值

NULL。

## 适用协议

RTSP。

## 可用性

在7.20.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。