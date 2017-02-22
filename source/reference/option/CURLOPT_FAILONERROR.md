## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_FAILONERROR, long fail);
```

## 概要

请求在遇到HTTP响应大于等于400时失败。

## 详情

一个`long`参数，设置成1时，告诉libcurl当返回的HTTP状态码等于或者大于400时让请求失败。默认的行为会正常返回页面，忽略那个状态码。

这个方法并不是可靠的，在有些情况下非成功的响应状态码会溜掉，特别是在身份验证的时候（响应状态码401和407）。

你可能会在检测到这种情况之前得到一些传输过来的头部，比如当收到“100-contintue”作为一个POST/PUT的响应时，之后会立刻收到401或者407。

## 默认值

0，遇到错误时不会失败。

## 适用协议

HTTP。

## 可用性

跟随HTTP。

## 返回值

如果启用了HTTP，返回`CURLE_OK`；否则返回`CURLE_UNKNOWN_OPTION`。