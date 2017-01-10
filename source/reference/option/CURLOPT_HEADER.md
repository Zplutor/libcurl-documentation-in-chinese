## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_HEADER, long onoff);
```

## 概要

把头部传递给数据流。

## 详情

对`onoff`传入1告诉libcurl在该`handle`上的请求的正文输出中包含头部。这个选项对于具有头部或者元数据的协议（像HTTP和FTP）来说是有意义的。

当要求像正文一样从相同的回调获取头部信息时，在没有详细了解正在使用的协议的情况下，不可能准确地把它们再次分开。

使用`CURLOPT_HEADERFUNCTION`来单独获取头部数据通常会更好。

> 疑问
>  
> * 设置了这个选项之后，`CURLOPT_HEADERFUNCTION`还有效吗？

虽然命名令人困惑地相似，但`CURLOPT_HTTPHEADER`是用来设置自定义HTTP头部的！

## 默认值

0

## 适用协议

大部分。

## 返回值

返回`CURLE_OK`。