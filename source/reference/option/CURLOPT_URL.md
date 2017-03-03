## 声明

```
CURLcode curl_easy_setopt(CURL *handle, CURLOPT_URL, char *URL);
```

## 概要

提供在请求中使用的URL。

## 详情

传入一个指向`URL`的指针来使用。该参数应该是一个指向以0字符结尾的`char*`，并且必须以下面的格式进行URL编码：

```
scheme://host:port/path
```

请参考RFC 3986获取对这个格式更好的解释。

在发起传输之前，libcurl不会验证语法或者使用这个变量。即使你在这里设置了一个疯狂的值，`curl_easy_setopt`仍然会返回`CURLE_OK`。

如果给定的URL缺少模式（例如“http://”或者“ftp://”等），libcurl会尝试基于下列给定的主机名称来解析协议：HTTP，FTP，DICT，LDAP，IMAP，POP3或SMTP。

不论是模式指定的协议，还是libcurl从主机名称推导出来的协议，如果它不被libcurl支持，那么当你在调用`curl_easy_perform`或`curl_multi_perform`函数时会返回`CURLE_UNSUPPORTED_PROTOCOL`。使用`curl_version_info`来获取关于哪些协议被你正在使用的libcurl版本支持的详细信息。

`CURLOPT_PROTOCOLS`可以用来限制libcurl在传输时使用哪些协议，这独立于编译libcurl时设置被支持的协议。当你从外部来源获取了一个URL并且想限制它的可访问性时，这可能会有用。

`CURLOPT_URL`是唯一一个必须在传输开始前设置的选项。

URL的主机部分包含了你想要连接的服务器的地址。这可以是一个完全限定的服务器域名，你网络上的机器的局域网名称，或者以IPv4或IPv6地址表示的服务器或机器的IP地址。例如：

```
http://www.example.com/
http://hostname/
http://192.168.0.1/
http://[2001:1890:1112:1::20]/
```

当使用下列的协议连接需要验证的服务器时，指定用户名，密码以及任何支持的登录选项作为主机的一部分也是可能的：

```
http://user:password@www.example.com
ftp://user:password@ftp.example.com
smb://domain%2fuser:password@server.example.com
imap://user:password;options@mail.example.com
pop3://user:password;options@mail.example.com
smtp://user:password;options@mail.example.com
```

目前只有IMAP，POP3和SMTP支持登录选项作为主机的一部分。关于在URL中登录选项语法的更多信息，请参考RFC 2384，RFC 5092和IETF草案draft-earhart-url-smtp-00.txt（在7.31.0加入）。

端口是可选的，当没有指定的时候libcurl会基于测定或指定的协议使用默认端口：HTTP是80，FTP是21以及SMTP是25，等等。下面的例子显示了如何指定端口：

```
http://www.example.com:8080/
```

这会使用8080端口而不是80来连接网页服务器。

```
smtp://mail.example.com:587/
```

这会在其它邮件端口上连接SMTP服务器。

URL的路径部分是协议特定的，下面的列表给出了一些例子，这个列表并非最终确定的：

### HTTP

HTTP请求的路径部分指定了要获取的文件以及从哪个目录获取。如果没有指定目录，那么会使用网页服务器的根目录。如果省略了文件，那么会获取指定目录或者根目录中的默认文档。每一个URL返回的确切的资源完全依赖于服务器的配置。

```
http://www.example.com
```

这会从网页服务器获取主页。

```
http://www.example.com/index.html
```

这会通过显式请求来返回主页。

```
http://www.example.com/contactus/
```

这会从contactus目录返回默认文档。

### FTP

FTP请求的路径部分指定了要获取的文件以及从哪个目录获取。如果文件部分被省略，那么libcurl会下载指定目录的目录清单。如果目录被省略，那么会返回根目录`/`的目录清单。

```
ftp://ftp.example.com
```

这会获取根目录的目录清单。

```
ftp://ftp.example.com/readme.txt
```

这会从根目录下载`readme.txt`文件。

```
ftp://ftp.example.com/libcurl/readme.txt
```

这会从`libcurl`目录下载`readme.txt`文件。

```
ftp://user:password@ftp.example.com/readme.txt
```

这会从用户的主目录获取`readme.txt`文件。当指定了用户名和密码时，在路径部分指定的所有东西都是相对于用户的主目录。为了从根目录或者根目录下面的目录中获取文件，需要在路径的开头添加一个额外的正斜杠来指定一个绝对路径。

```
ftp://user:password@ftp.example.com//readme.txt
```

这会以指定的用户来登录，并且从根目录获取`readme.txt`文件。

### SMTP

SMTP请求中的路径部分指定了在与邮件服务器通信过程中显示的主机名。如果路径被省略，那么libcurl会尝试去解析本地计算机的主机名。然而，这可能不会返回有些邮件服务器所要求的完全限定域名，而通过指定这个路径，你可以设置一个另外的名称，例如你的机器的完全限定域名，这个域名可能是从`gethostname`或`getaddrinfo`等外部函数得来的。

```
smtp://mail.example.com
```

这会连接`example.com`上的邮件服务器，并且在`HELO`/`EHELO`命令中发送你本地计算机的主机名。

```
smtp://mail.example.com/client.example.com
```

这会在`HELO`/`EHLO`命令中发送`client.example.com`给`example.com`上的邮件服务器。

### POP3

POP3请求中的路径部分指定了要获取的消息ID。如果没有指定ID，那么会返回一份正在等待的消息列表。

```
pop3://user:password@mail.example.com
```

这会列出该用户可拉取的消息。

```
pop3://user:password@mail.example.com/1
```

这会获取该用户的第一条消息。

### IMAP

IMAP请求中的路径部分不仅可以指定要列清单（在7.30.0加入）或选择的邮箱，也可以用来检查邮箱的UIDVALIDITY，指定消息的UID，SECTION（在7.30.0加入）和PARTIAL位组（在7.37.0加入）来拉取，以及指定搜索哪些消息（在7.37.0加入）。

```
imap://user:password@mail.example.com
```

执行顶层目录列清单。

```
imap://user:password@mail.example.com/INBOX
```

在用户的收件箱中执行列目录清单。

```
imap://user:password@mail.example.com/INBOX/;UID=1
```

选择用户的收件箱并且拉取消息1。

```
imap://user:password@mail.example.com/INBOX;UIDVALIDITY=50/;UID=2
```

选择用户的收件箱，检查邮箱的UIDVALIDITY是否50，如果是的话拉取消息2。

```
imap://user:password@mail.example.com/INBOX/;UID=3/;SECTION=TEXT
```

选择用户的收件箱并且拉取消息3的文本部分。

```
imap://user:password@mail.example.com/INBOX/;UID=4/;PARTIAL=0.1024
```

选择用户的收件箱并且拉取消息4的第一个1024位组。

```
imap://user:password@mail.example.com/INBOX?NEW
```

选择用户的收件箱并且检查NEW消息。

```
imap://user:password@mail.example.com/INBOX?SUBJECT%20shadows
```

选择用户的收件箱并且搜索标题行中包含“shadows”的消息。

关于一个IMAP URL中每个部分的更多信息，请参阅RFC 5092。

### SCP

SCP请求中的路径部分指定要获取的文件以及从哪个目录获取。文件部分不能省略。文件以基于服务器上根目录的绝对路径来获取。为了指定一个相对于服务器上用户主目录的路径，需要在路径部分前置`~/`。如果用户名没有内嵌在URL中，可以通过`CURLOPT_USERPWD`和`CURLOPT_USERNAME`选项来设置。

```
scp://user@example.com/etc/issue
```

这指定了`/etc/issue`文件。

```
scp://example.com/~/my-file
```

这指定了服务器上用户主目录中的`my-file`文件。

### SFTP

SFTP请求中的路径部分指定了要获取的文件以及从哪个目录获取。如果省略了文件部分，那么libcurl会下载指定目录的目录清单。如果路径以`/`结束，那么会返回目录清单而不是文件。如果路径完全省略，那么会返回根`/`目录的目录清单。如果用户名没有内嵌在URL中，那么可以通过`CURLOPT_USERPWD`或者`CURLOPT_USERNAME`选项来设置。

```
sftp://user:password@example.com/etc/issue
```

这指定了`/etc/issue`文件。

```
sftp://user@example.com/~/my-file
```

这指定了在用户主目录中的`my-file`文件。

```
sftp://ssh.example.com/~/Documents/
```

这会请求用户主目录下`Documents`目录的目录清单。

### SMD

SMB请求中的路径部分指定了要从哪个共享目录获取哪个文件，或者要上传到哪个共享目录，在这种情况下，路径不能省略。如果用户名没有内嵌在URL中，那么可以通过`CURLOPT_USERPWD`或者`CURLOPT_USERNAME`选项来设置。如果用户名内嵌在URL中，那么它必须包含域名，而且反斜杠必须进行URL编码成`%2f`。

```
smb://server.example.com/files/issue 
```

这指定了位于`files`共享目录下的`issue`文件。

```
smb://server.example.com/files/ -T issue
```

这指定了将会上传到`files`共享目录下的`issue`文件。

### LDAP

LDAP请求中的路径部分可以用来指定LADP搜索的识别名，属性，作用域，过滤器以及扩展名。每个字段用问号分隔，如果一个字段不是必须的，那么应该包含问号和空字符串。

```
ldap://ldap.example.com/o=My%20Organisation
```

这会用`My Organisation`作为DN执行一次LDAP搜索。

```
ldap://ldap.example.com/o=My%20Organisation?postalAddress
```

这会执行跟上例一样的搜索，但只会返回`postalAddress`属性。

```
ldap://ldap.example.com/?rootDomainNamingContext
```

这指定了空的DN，并且请求活动目录服务器上关于`rootDomainNamingContext`属性的信息。

关于LDAP URL中每个部分的信息，请参阅RFC 4516。

### RTMP

RTMP没有正式的URL规范，所以libcurl使用内部的librtmp库支持的URL语法。它的语法是，在传统URL后面接上一个空格和一系列用空格分隔的名称=值对。

空格通常不是一个“合法”的字符，但libcurl会接受它。当用户想传入一个`#`字符时，如果它以字面值输入，那么它会被认为是一个分片，从而被libcurl裁剪掉。你必须将它转义成反斜杠以及它的十六进制ASCII编码值：`\23`。

## 默认值

没有默认的URL。如果这个选项没有设置，那么不会执行任何传输。

## 安全性

应用程序有时可能会发现，允许用户为了各种目的而设定URL很方便。这个字符串最终会设置到这个选项中。

从外部未信任的地方获取URL会带来一些安全性担忧的原因：

如果你有一个应用程序作为服务应用程序，或者在服务器上运行，拿到一个未经过滤的URL会容易欺骗你的程序去访问一个本地的资源而不是远程的。当接受用户提供的URL时，针对本机访问来保护你自己是很困难的，

这些自定义的URL也可能会访问其它端口，而不是你计划好的在普通URL格式中的端口。本地主机和自定义端口的组合会让外部用户对你的本地服务器耍诡计。

接受外部URL也可能会使用了其它协议而不是http或者其它常用协议。可以通过`CURLOPT_PROTOCOLS`来限制接受哪种协议。

用户提供的URL也可能会指向一个重定向到其它位置的位置（同样可能会重定向到其它协议）。再考虑一下你的`CURLOPT_FOLLOWLOCATION`和`CURLOPT_REDIR_PROTOCOLS`选项。

## 适用协议

所有协议。

## 可用性

POP3和SMTP在7.31.0加入。

## 返回值

成功的话返回`CURLE_OK`；或者如果没有足够堆空间的话返回`CURLE_OUT_OF_MEMORY`。

注意`curl_easy_setopt`不会解析给定的URL，所以如果传入一个无效的URL，它不会被检查出来，直到`curl_easy_perform`或者类似的函数被调用。