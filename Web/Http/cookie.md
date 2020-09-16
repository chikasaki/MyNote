`cookie`与`session`:
1. `cookie`是什么:
    - 是服务器使用`SET-COOKIE`响应头设置的一组键值对
    - 客户端在请求服务器时，会将这些`cookie`一并带上
    
2. `SET-COOKIE`的一些属性:
    - expire: cookie过期时间
    - max-age: cookie保持时间
    - HttpOnly: 设置之后js脚本不能访问到此cookie
    - Domain
    
3. cookie域名设置:
    - cookie不可跨域名，使用A域名cookie去访问B域名是无用的
    - 低级域名可以使用高级域名的cookie，比如:
        - 二级域名: `www.baidu.com` 和 `image.baidu.com`都
        可以使用一级域名: `baidu.com`下的cookie

4. 一般会使用`cookie`来实现`session`

5. `cookie`的初衷:
    - `http`是无状态的协议，意味着当需要某些认证信息才能访问资源时，
    每次请求都要让用户手动输入这些认证信息
    - `cookie`是简化机制，可以让浏览器发送请求时，自动带上这些认证
    信息，来避免繁琐的操作