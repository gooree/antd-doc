# Service

Service 负责与 HTTP 接口对接，进行纯粹的数据读写。

## request.js

service组件中使用request函数发起http请求，其函数原型为：

```
/**
 * Requests a URL, returning a promise.
 *
 * @param  {string} url       The URL we want to request
 * @param  {object} [options] The options we want to pass to "fetch"
 * @return {object}           An object containing either "data" or "err"
 */
export default async function request(url, options) {
  const response = await fetch(url, options);
  checkStatus(response);
  return await response.json();
}
```

> 注：该函数第一个参数为后端服务接口的url，第二个参数为请求的配置对象（有点类似于jquery的ajax请求）。

常见的用法如下：
```
return request('/api/cards/add', {
    headers: {
      'content-type': 'application/json',
    },
    method: 'POST',
    body: JSON.stringify(data),
});
```

> 注：如果不提供options参数，则默认请求方法为GET请求。