## 声明

```
CURLcode curl_global_init_mem(
	long flags, 
	curl_malloc_callback m, 
	curl_free_callback f, 
	curl_realloc_callback r, 
	curl_strdup_callback s, 
	curl_calloc_callback c); 
```

## 概要

带有内存回调的全局libcurl初始化。

## 详情

该函数的行为与`curl_global_init`完全一致，除了一点：它允许应用程序设置回调来替换内部使用的内存函数。

这个页面只添加了关于回调的文档，参考`curl_global_init`的页面来获取其它信息。当你使用该函数时，所有回调参数必须设置成有效的函数指针。

回调的原型应该匹配下面这些：

```
void *malloc_callback(size_t size);
```

替换`malloc()`

```
void free_callback(void *ptr);
```

替换`free()`


```
void *realloc_callback(void *ptr, size_t size);
```

替换`realloc()`


```
char *strdup_callback(const char *str);
```

替换`strdup()`

```
void *calloc_callback(size_t nmemb, size_t size);
```

替换`calloc()`

## 警告

操纵这些回调给应用程序提供了相当大的能力去严重破坏libcurl。务必要小心！


