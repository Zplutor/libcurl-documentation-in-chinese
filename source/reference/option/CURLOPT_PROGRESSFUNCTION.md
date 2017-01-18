## 声明

```
int progress_callback(void *clientp,   double dltotal,   double dlnow,   double ultotal,   double ulnow);

CURLcode curl_easy_setopt(CURL *handle, CURLOPT_PROGRESSFUNCTION, progress_callback);
```

## 概要

进度计量函数的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该匹配上面显示的原型。

我们鼓励用户使用更新的`CURLOPT_XFERINFOFUNCTION`，如果可以的话。

libcurl会以频繁的间隔来调用该函数，而不是调用它内部的进度计量函数。当数据正在传输的时候它会被调用地很频繁，而在慢速阶段，例如没有东西在传输的时候，它的调用频率会下降到大约每秒一次。

`clientp`是通过`CURLOPT_PROGRESSDATA`设置的指针，它不会被libcurl使用，而仅仅是从应用程序传递给回调。

该回调会被告知有多少数据libcurl将会传输以及已经传输，以字节为单位。`dltotal`是libcurl期望在这个传输中下载的总字节数。`dlnow`是至今已经下载的总字节数。`ultotal`是libcurl期望在这个传输中上传的总字节数。`ulnow`是至今已经上传的总字节数。

未知或者不使用的参数值会设置成0来传递给回调（例如当你只下载数据时，上传大小总是为0）。很多时候，在libcurl知道数据大小之前，这个回调首先会被调用一次或多次，因此程序必须能够处理这种情况。

从这个回调返回一个非0值会导致libcurl终止传输，并返回`CURLE_ABORTED_BY_CALLBACK`。

如果你使用multi接口来传输数据，这个函数不会在空闲阶段被调用，除非你调用了恰当的会进行传输的libcurl函数。

为了让这个函数能够被真正调用，`CURLOPT_NOPROGRESS`必须设置成0。

## 默认值

默认情况下，libcurl有一个内部的进度计量。这很少是用户想要的。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。