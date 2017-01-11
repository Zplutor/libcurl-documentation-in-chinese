## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_NOSIGNAL, long onoff);
```

## 概要

跳过所有信号处理。

## 详情

如果`onoff`的值是1，libcurl不会使用任何安装信号处理器的函数，或者任何给进程发送信号的函数。该选项的目的是让多线程的UNIX应用程序仍然可以设置/使用所有像超时这样的选项，而不必冒触发信号的风险。

如果设置了这个选项，并且libcurl使用标准的名称解析器来构建，那么在解析名称的时候不会发生超时。考虑在构建libcurl时用c-ares或者使用线程的解析器后端，来开启异步的DNS查找，以及在不使用信号的情况下开启名称解析超时。

把`CURLOPT_NOSIGNAL`设置成1使得libcurl不会要求系统忽略SIGPIPE信号，不然系统会在试图向一个已经被另一端关闭的套接字发数据的时候发送这个信号。libcurl尽量不会让这种SIGPIPE信号触发，但是在一些操作系统上没有办法避免它们，甚至在那些可以避免的系统上也有一些边缘情况仍然可能会导致信号发生，与我们的期望相反。另外，使用`CURLAUTH_NTLM_WB`身份验证可能会导致一个SIGCHLD信号触发。

## 默认值

0

## 可用性

在7.10中添加。

## 返回值

如果支持该选项的话，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。