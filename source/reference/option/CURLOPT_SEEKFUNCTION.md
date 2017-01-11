## 声明

```
/* These are the return codes for the seek callbacks */
#define CURL_SEEKFUNC_OK       0
#define CURL_SEEKFUNC_FAIL     1 /* fail the entire transfer */
#define CURL_SEEKFUNC_CANTSEEK 2 /* tell libcurl seeking can't be done, so
                                    libcurl might try other means instead */
 
int seek_callback(void *userp, curl_off_t offset, int origin);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SEEKFUNCTION, seek_callback);
```

## 概要

在输入流中重定位的用户回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该匹配上面显示的原型。

libcurl需要定位到输入流的一个确切位置时会调用这个函数，以及在恢复上传的时候用这个函数来快速前进（而不是通过普通的读取函数/回调来读取所有已经上传过的字节）。当数据已经发送到服务器上并且需要再次发送时，它也会被调用来回溯一个流。这可能会发生在以下情况下：当以多通道的身份验证方式执行一个HTTP PUT或者POST时；或者当一个已存在的HTTP连接重用得太晚而服务器已经关闭了这个连接时。该函数的作用应该类似`fseek`或者`lseek`，它的`origin`参数会被设置成`SEEK_SET`，`SEEK_CUR`或者`SEEK_END`，虽然libcurl目前只传入`SEEK_SET`。

`userp`是你通过`CURLOPT_SEEKDATA`设置的指针。

回调函数必须返回`CURL_SEEKFUNC_OK`以示成功。返回`CURL_SEEKFUNC_FAIL`则会导致上传操作失败。或者返回`CURL_SEEKFUNC_CANTSEEK`指示libcurl虽然重定位失败，但是它可以在可能的情况下自行处理这个问题。后者有时可以通过读取输入或者其它类似的方法来解决。

如果你把输入参数直接转发给`fseek`或者`lseek`，要注意`offset`的数据类型在很多系统上与`curl_off_t`的定义不一样！

## 默认值

默认情况下，这个值是NULL，没有使用。

## 适用协议

HTTP，FTP，SFTP

## 可用性

在7.18.0加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。