---
title: ajax跨域调用
date: 2017-02-12 15:16:53
categories:
- 问题
tags:
- 问题
- java
---

## ajax跨域调用

#### 跨域

###### 客户端ajax请求代码

```javascript
<script language="JavaScript">
        $(document).ready(function () {
            $('#test').click(function () {
                console.log("点击按钮")
                $.ajax({
                    type: 'get',
                    async: false,
                    url: 'http://localhost:9999/api/web/test/table',
                    dataType: 'jsonp',
                    jsonp: 'callback',
                    jsonpCallback: 'success_jsonpCallback',
                    success: function (json) {
                        console.log(json);
                    },
                    error: function(){
                        console.log('error');
                    }
                });
            });
        });
    </script>
```

###### 服务端代码

- 服务端配置代码
```java
package com.xy.demo.common.config;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.AbstractJsonpResponseBodyAdvice;

/**
 * jsonp配置，可将含有callback参数的请求返回jsonp格式的application/JavaScript数据
 */
@ControllerAdvice(basePackages = "com.xy.demo.controller")
public class JsonpAdvice extends AbstractJsonpResponseBodyAdvice{
    public JsonpAdvice() {
        super("callback");
    }
}
```

- 服务端api代码
```java
    @RequestMapping(value = "/web/test/table", produces = MediaType.APPLICATION_JSON_VALUE)
    @ResponseBody()
    public Map testTable() {
        Map<String, String> map = new HashMap<>();
        map.put("name", "xingyun");
//      JSONPObject jsonpObject = new JSONPObject(callback, map); // 这种方式返回数据类型为json而jsonp需要javascrip数据
        return map;
    }
```


