1. 什么是跨域: 
    - 浏览器的默认安全策略，当两个不同域名之间互相访问时，如果
    不做一些措施，可能会因为浏览器的策略导致我们看不到渲染出来的数据
    
2. Http几个关于跨域访问的请求头:
    - Access-Control-Allow-Origin:  服务器返回的允许访问的请求域名
    - Access-Control-Allow-Headers: 允许的请求头
    - Access-Control-Allow-Methods: 允许的请求方法
    - Access-Control-Max-Age:       上面三个请求头的生效时间
    
3. img, link, script等html标签中的src属性，是不受跨域影响的

4. 简单请求与非简单请求:
    - 简单请求:
        - 请求方法: one of {HEAD, GET, POST}
        - 只能包含以下Http请求头或相应属性:
            - Accept
            - Accept-Language
            - Content-Language
            - Last-Event-ID
            - Content-Type:
                - application/x-www-form-urlencoded
                - multipart/form-data
                - text/plain
    - 非简单请求: 不满足以上条件的就是非简单请求
    - 简单请求与非简单请求在浏览器中的策略:
        - 简单请求直接请求服务器
        - 非简单请求会先发送**预检请求**，通过预检请求之后
        才会发送真正的请求
        - 预检请求与真正请求的区别:
            - 预检请求是一个不包含实质内容的请求，一般是
            让服务器检查真正请求的`Origin`，`Headers`，`Methods`
            是否满足条件
            
          