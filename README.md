## ws + tls 

使用镜像 挂载。

```
git clone https://github.com/comeyna/reality
docker build -t xray:reality .
```

```
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 443,
      "protocol": "vless",
      "settings": {
        "clients": [
          {
            "email": "ws+tls",
            "flow": "",
            "id": "354b0b63-8ae1-43fe-b343-e999be5194c7"
          }
        ],
        "decryption": "none",
        "fallbacks": []
      },
      "streamSettings": {
        "network": "ws",
        "security": "none",
        "wsSettings": {
          "acceptProxyProtocol": false,
          "headers": {},
          "heartbeatPeriod": 0,
          "host": "",
          "path": "/rss"
        }
      },
      "tag": "inbound-50002",
      "sniffing": {
        "enabled": false,
        "destOverride": [
          "http",
          "tls",
          "quic",
          "fakedns"
        ],
        "metadataOnly": false,
        "routeOnly": false
      },
      "allocate": {
        "strategy": "always",
        "refresh": 5,
        "concurrency": 3
      }
    }
  ]
```

## 挂载到 nignx

```
    # 配置 ws+tls 节点 优化版
    location /rss {
        proxy_redirect off;
        # 反向代理
        proxy_pass http://127.0.0.1:50002;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        # WebSocket
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
		
        # 禁用缓存（WebSocket 通常不需要缓存）
        proxy_no_cache 1;
        proxy_cache_bypass 1;

        # 增强 WebSocket 连接稳定性
        proxy_buffering off;  # 禁用缓冲，WebSocket 协议不需要缓冲
        tcp_nodelay on;       # 开启 TCP_NODELAY，减少延迟
        tcp_nopush on;        # 开启 TCP_NOPUSH，减少传输中的数据碎片

        # 调整缓冲区大小
        proxy_buffer_size 16k;
        proxy_buffers 8 16k;
        proxy_max_temp_file_size 0;  # 禁用磁盘临时文件，改为内存缓冲		
    }
```

## 配置单

```
```
