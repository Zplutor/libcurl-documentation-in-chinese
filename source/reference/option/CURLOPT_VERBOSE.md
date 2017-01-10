## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_VERBOSE, long onoff);
```

## 概要

设置详情模式的开或关。

## 详情

把`onoff`参数设置成1使得libcurl显示很多关于这个`handle`上的操作的详细信息。这对于libcurl以及协议的调试和理解很有帮助。详细信息将会输出到stderr，或者通过`CURLOPR_STDERR`设置的流。

你不会想要在正式产品中设置这个选项，你总是在调试/报告问题的时候需要它。

如果要同时获得所有收发的协议数据，可以考虑使用`CURLOPT_DEBUGFUNCTION`。

## 默认值

0，意味着关闭该选项。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。