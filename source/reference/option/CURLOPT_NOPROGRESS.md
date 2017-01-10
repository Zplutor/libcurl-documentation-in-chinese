## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_NOPROGRESS, long onoff);
```

## 概要

关闭进度计量。

## 详情

如果`onff`被设置成1，它告诉libcurl完全关闭该`handle`上的请求的进度计量。它也会阻止`CURLOPT_PROGRESSFUNCTION`被调用。

libcurl的未来版本可能完全不会带有任何内置的进度计量。

> 疑问
> * 进度计量的确切意义是什么？目前libcurl内置的进度计量是怎样的？

## 默认值

1，意味着句柄通常不带有任何进度计量来执行。

## 返回值

返回`CURLE_OK`。