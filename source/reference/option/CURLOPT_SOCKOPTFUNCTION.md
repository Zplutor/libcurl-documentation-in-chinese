## 声明

```
typedef enum  {
  CURLSOCKTYPE_IPCXN,  /* socket created for a specific IP connection */
  CURLSOCKTYPE_ACCEPT, /* socket created by accept() call */
  CURLSOCKTYPE_LAST    /* never use */
} curlsocktype;
 
#define CURL_SOCKOPT_OK 0
#define CURL_SOCKOPT_ERROR 1 /* causes libcurl to abort and return
                                CURLE_ABORTED_BY_CALLBACK */
#define CURL_SOCKOPT_ALREADY_CONNECTED 2
 
int sockopt_callback(void *clientp,
                     curl_socket_t curlfd,
                     curlsocktype purpose);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_SOCKOPTFUNCTION, sockopt_callback);
```

## 概要

设置套接字选项的回调。

## 详情

传入一个你的回调函数的指针，该函数应该匹配上面显示的原型。

设置了这个选项后，在创建套接字之后以及发起连接之前，libcurl会调用这个回调函数，以此允许应用程序修改指定的套接字选项。回调的`purpose`参数标识了这个特定套接字的确切用途：

`CURLSOCKTYPE_IPCXN`标识主动创建的连接；从7.28.0开始，`CURLSOCKTYPE_ACCEPT`标识用PORT/EPSV配置的FTP连接（在更早的版本上这些套接字不会传递给这个回调）。

未来的libcurl版本可能会支持更多用途。libcurl把刚刚创建的套接字描述符传递给回调的`curlfd`参数，所以在用户的自行判断下可以调用额外的`setsockopt`。

`clientp`指针包含了任何通过`CURLOPT_SOCKOPTDATA`选项设置的用户定义的值。

回调返回`CURL_SOCKOPT_OK`表示成功。回调返回`CURL_SOCKOPT_ERROR`会标识一个不可恢复的错误给libcurl，并且会关闭套接字以及返回`CURLE_COULDNT_CONNECT`。另外，回调函数可以返回`CURL_SOCKOPT_ALREADY_CONNECTED`，告诉libcurl这个套接字已经连接了，这样libcurl不会试图去连接它。这允许一个应用程序通过`CURLOPT_OPENSOCKETFUNCTION`传入一个已经连接的套接字，然后用这个函数让libcurl不再试图（再次）连接。

> 备注
>
> * 这里的意思是，应用程序可以在`CURLOPT_OPENSOCKETFUNCTION`回调中创建一个已经连接好的套接字，然后在`CURLOPT_SOCKOPTFUNCTION`回调中返回`CURL_SOCKOPT_ALREADY_CONNECTED`，让libcurl不再连接这个套接字。

## 默认值

默认情况下，这个回调是NULL，不使用。

## 适用协议

所有协议。

## 可用性

在7.16.0加入。`CURL_SOCKOPT_ALREADY_CONNECTED`返回值在7.21.5加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。