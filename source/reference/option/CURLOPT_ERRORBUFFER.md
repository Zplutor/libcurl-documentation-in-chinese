## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_ERRORBUFFER, char *buf);
```

## 概要

为错误信息设置错误缓冲区。

## 详情

传入一个指向缓冲区的`char*`，当发生错误或者问题的时候libcurl会存储人类可读的错误信息到这个缓冲区中。这可能比`curl_easy_perform`以及其它相关函数的单一返回值更有帮助。

你必须保持相关的缓冲区有效，直到libcurl不再需要它。不这么做的话会导致非常怪异的行为，或者甚至崩溃。libcurl会需要它，直到你调用了`curl_easy_cleanup`或者你再次设置同样的选项来使用一个不同的指针。

考虑使用`CURLOPT_VERBOSE`和`CURLOPT_DEBUGFUNCTION`来更好地调试和追踪为什么错误会发生。

如果libcurl不返回错误，这个缓冲区可能不会被修改。在这种情况下不要依赖它的内容。

## 默认值

NULL。

## 适用协议

所有协议。

## 可用性

总是可用。

## 返回值

返回`CURLE_OK`。