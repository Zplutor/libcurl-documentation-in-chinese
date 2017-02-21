## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_INTERLEAVEDATA, void *pointer);
```

## 概要

传给RTSP交叉回调的自定义指针。

## 详情

这是在接收到交叉RTP数据时传递给`CURLOPT_INTERLEAVEFUNCTION`的`userdata`指针。

## 默认值

NULL。

## 适用协议

RTSP。

## 可用性

在7.20.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。