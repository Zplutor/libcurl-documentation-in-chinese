## 声明

```
typedef enum  {
  CURLSOCKTYPE_IPCXN,  /* socket created for a specific IP connection */
  CURLSOCKTYPE_ACCEPT, /* socket created by accept() call */
  CURLSOCKTYPE_LAST    /* never use */
} curlsocktype;
 
struct curl_sockaddr {
  int family;
  int socktype;
  int protocol;
  unsigned int addrlen;
  struct sockaddr addr;
};
 
curl_socket_t opensocket_callback(void *clientp,
                                  curlsocktype purpose,
                                  struct curl_sockaddr *address);
 
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_OPENSOCKETFUNCTION, opensocket_callback);
```

## 概要

设置打开套接字的回调。

## 详情

传入一个指向你的回调函数的指针，该函数应该匹配上面显示的原型。

该回调函数会替代`socket`被libcurl调用。回调的`purpose`参数标识了这个特定套接字的确切用途：`CURLSOCKTYPE_IPCXN`标识基于IP的连接，而`CURLSOCKTYPE_ACCEPT`标识调用`accept`之后创建的套接字——例如正在执行活动的FTP。未来的libcurl版本会支持更多的用途。

`clientp`指针包含任何通过`CURLOPT_OPENSOCKETDATA`函数设置的用户定义值。

该回调通过`address`参数得到已解析的对端地址，并且允许修改这个地址或者完全拒绝连接。回调函数应该返回新创建的套接字，或者在没有连接可以建立或检测到其它错误的情况下返回`CURL_SOCKET_BAD`。当然，根据用户的意愿，任何额外的`setsockopt`也可以被用户调用。从回调函数返回的`CURL_SOCKET_BAD`返回值会标识一个不可恢复的错误给libcurl，并且会从触发这个回调的函数返回`CURLE_COULDNT_CONNECT`。这个返回值可以用在IP地址黑名单上。

如果你想传入一个已经建立了连接的套接字，可以通过这个回调传回该套接字，然后使用`CURLOPT_SOCKOPTFUNCTION`标识这个套接字是已经连接了的。

## 默认值

默认的行为相当于下面所示：

```
return socket(addr->family, addr->socktype, addr->protocol);
```

## 适用协议

所有协议。

## 可用性

在7.17.1加入。

## 返回值

如果支持该选项，返回`CURLE_OK`，否则返回`CURLE_UNKNOWN_OPTION`。