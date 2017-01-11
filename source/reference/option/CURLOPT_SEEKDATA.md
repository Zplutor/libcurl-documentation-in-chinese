## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SEEKDATA, void *pointer);
```

## 概要

传递给重定位回调的自定义指针。

## 详情

设置传递给重定位回调函数的数据`pointer`。如果你使用了`CURLOPT_SEEKFUNCTION`选项，你会得到这个作为输入的指针。

## 默认值

如果你没有设置这个选项，NULL会传递给回调。

## 适用协议

HTTP，FTP，SFTP

## 可用性

在7.18.0加入。
