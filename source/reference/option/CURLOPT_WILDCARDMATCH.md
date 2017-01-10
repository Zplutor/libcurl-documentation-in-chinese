## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_WILDCARDMATCH, long onoff);
```

## 概要

开启文件夹通配符传输。

## 详情

如果你想要根据一个文件名模式传输多个文件，把`onoff`设置成1。这个模式可以作为`CURLOPT_URL`选项的部分来指定，即在URL的最后部分（文件名）使用一个类似`fnmatch`的模式（Shell模式匹配）。

默认情况下，libcurl使用它自己内部的通配符匹配实现。你可以通过`CURLOPT_FNMATCH_FUNCTION`选项提供你自己的匹配函数。

以下是它的语法的简单介绍：

### * - 星号

`ftp://example.com/some/path/*.txt`（根目录下的所有txt）

### ? - 问号

问号匹配任意（一个）字符。

`ftp://example.com/some/path/photo?.jpeg`

### [ - 方括号表达式

左方括号开启一个方括号表达式。问号和星号在方括号表达式中没有特殊意义。每个方括号表达式以右方括号结束，并且只匹配一个字符。下面是一些例子：

`[a-zA-Z0-9]`或`[f-gF-G]` - 字符区间

`[abc]` - 字符枚举

`[^abc]`或`[!abc]` - 取反

`[[:name:]]` - 分类表达式。支持的分类有`alnum`，`lower`，`space`，`alpha`，`digit`，`print`，`upper`，`blank`，`graph`，`xdigit`。

`[][-!^]` - 特殊情况 - 只匹配`-`，`]`，`[`，`!`或`^`。这些字符没有特别用途。

`[\[\]\\]` - 转义语法。匹配`[`，`]`或`\`。

利用上面的规则，一个文件名模式可以构造成：

`ftp://example.com/some/path/[a-z[:upper:]\\].jpeg`

## 适用协议

这个特性只支持FTP下载。

##可用性

在7.21.0添加。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。