# MQTT 测试流程

## 使用 EMQX 搭建服务

### docker
~~~
docker run -d --name emqx -p 1883:1883 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx/emqx:5.0.11
~~~

### 端口说明

* `1883` - tcp 接口
* `8083` - ws 订阅接口
* `8084` - wss 订阅接口
* `8883` - ssl 接口
* `18083`     - 后台管理面板 默认 `admin` : `public`


### nginx ws端口代理

~~~
# 代理
upstream emqx_wss {
  server 127.0.0.1:8084 max_fails=2 fail_timeout=10s weight=1;
}

# 优雅处理 Connection
map $http_upgrade $connection_upgrade {
 default upgrade;
 '' close;
}

location / {
  proxy_pass http://emqx_wss;
  
  proxy_set_header Host $host; #多层代理需要去掉
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

  # 下面为ws必须配置
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
	proxy_set_header Connection $connection_upgrade;
	proxy_set_header X-Real-IP $remote_addr;
}

location /ws/{
  # ...
  # proxy_pass http://emqx_wss/;
}
~~~

### 在线测试站点

[MQTT在线](http://www.emqx.io/online-mqtt-client#)


### 前端使用 `MQTT.js`
* `2.18.9` 版本推荐，使用错误版本可能出现打包问题`
~~~ 
yarn add mqtt@2.18.9
~~~