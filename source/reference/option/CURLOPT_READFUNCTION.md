## 声明

```
size_t read_callback(char *buffer, size_t size, size_t nitems, void *instream);

CURLcode curl_easy_setopt(CURL *handle, CURLOPT_READFUNCTION, read_callback);
```

## 概要

上传数据的读回调。

## 详情

传入一个指向你的回调函数的指针，与上面显示的原型一致。

一旦libcurl需要读数据发给另一端的时候，这个回调函数就会被调用——比如当你要求它上传或传递数据给服务器。你的函数应该填充`buffer`指向的数据区域，并且最多填充`size`乘以`nitems`个字节的数据。

然后你的函数必须返回实际存储到那块内存区域的字节数。返回0会标识文件结束给libcurl，并且会导致停止当前的传输。

如果你过早地返回0来停止当前传输（例如，在服务器期望结束之前，比如当你说你会上传N字节，但你上传的少于N字节），你可能会体验到服务器由于在等待剩下的那些不会到来的数据而挂起。

读回调可以返回`CURL_READFUNC_ABORT`来立即停止当前的操作，导致传输返回`CURLE_ABORTED_BY_CALLBACK`错误码。

回调可以返回`CURL_READFUNC_PAUSE`来暂停从这个连接读取。参考`curl_easy_pause`获取更多详情。

缺陷：当正在使用TFTP上传时，你必须返回该回调想要的确切的数据量，否则这批数据会被服务端认为是最后的包而停止传输。

如果你设置这个回调指针为NULL，或者根本不设置，那么内部默认的读函数会被使用。它会以通过`CURLOPT_READDATA`设置的FILE*来调用`fread`。

## 默认值

默认的内部读回调是`fread`。

## 适用协议

该选项适用于所有上传协议。

## 可用性

`CURL_READFUNC_PAUSE`返回值在7.18.0加入，以及`CURL_READFUNC_ABORT`在7.12.1加入。

## 返回值

该选项会返回`CURLE_OK`。