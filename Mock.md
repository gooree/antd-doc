# Mock

<p>在实际的开发中，后端的接口服务往往并不能马上可用，这就需要本地服务器具备另外一个能力：模拟数据（mock）。</p>

<p>一个 ajax 请求发送到本地开发服务器后，我们可以设置：如果请求满足某个规则，则不转发这个请求，而是直接返回一个「假」结果给浏览器。在实际的开发中，我们常常事先和服务端的同学商定http请求的接口接受什么参数，返回什么结果，然后先用 mock 数据来模拟，自己和自己「联调」。等服务端同学开发完成后，再解除 mock，用真实数据「联调」。</p>

## 模拟正常返回

在mock文件夹下新建js文件，整个文件需要export一个js对象，对象的key是由
```
<Http_Method> <Resource_Uri>
```
构成的，值是function，当一个ajax调用匹配了Key后，与之对应的function就会被执行。函数中我们调用res.json就可以给浏览器返回结果。

## 模拟出错

利用res.status也可以模拟http请求出错。

```
export default {
  'get /dev/random_joke': function (req, res) {
    res.status(500);
    res.json({});
  },
};
```