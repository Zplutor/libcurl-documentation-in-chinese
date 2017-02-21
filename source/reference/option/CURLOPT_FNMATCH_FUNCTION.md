## 声明

```
int fnmatch_callback(void *ptr,
                     const char *pattern,
                     const char *string);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_FNMATCH_FUNCTION,
                          fnmatch_callback);
```

## 概要

通配符匹配函数回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该符合上面显示的原型。

这个回调用来进行通配符匹配。

如果`pattern`匹配`string`，返回`CURL_FNMATCHFUNC_MATCH`；如果不匹配，返回`CURL_FNMATCHFUNC_NOMATCH`；如果有错误发生，返回`CURL_FNMATCHFUNC_FAIL`。

## 默认值

NULL，相当于内部的通配符匹配函数。

## 适用协议

FTP。

## 可用性

在7.21.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。