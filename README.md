## ws + tls 

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
