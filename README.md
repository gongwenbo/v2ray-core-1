# Move To https://github.com/v2fly/v2ray-core

***

# Project V

[![GitHub Test Badge][1]][2] [![codecov.io][3]][4] [![GoDoc][5]][6] [![codebeat][7]][8] [![Downloads][9]][10] [![Downloads][11]][12]

[1]: https://github.com/v2fly/v2ray-core/workflows/Test/badge.svg "GitHub Test Badge"
[2]: https://github.com/v2fly/v2ray-core/actions "GitHub Actions Page"
[3]: https://codecov.io/gh/v2fly/v2ray-core/branch/master/graph/badge.svg?branch=master "Coverage Badge"
[4]: https://codecov.io/gh/v2fly/v2ray-core?branch=master "Codecov Status"
[5]: https://godoc.org/v2ray.com/core?status.svg "GoDoc Badge"
[6]: https://godoc.org/v2ray.com/core "GoDoc"
[7]: https://goreportcard.com/badge/github.com/v2fly/v2ray-core "Goreportcard Badge"
[8]: https://goreportcard.com/report/github.com/v2fly/v2ray-core "Goreportcard Result"
[9]: https://img.shields.io/github/downloads/v2ray/v2ray-core/total.svg "v2ray/v2ray-core downloads count"
[10]: https://github.com/v2ray/v2ray-core/releases "v2ray/v2ray-core release page"
[11]: https://img.shields.io/github/downloads/v2fly/v2ray-core/total.svg "v2fly/v2ray-core downloads count"
[12]: https://github.com/v2fly/v2ray-core/releases "v2fly/v2ray-core release page"

Project V is a set of network tools that help you to build your own computer network. It secures your network connections and thus protects your privacy. See [our website](https://www.v2fly.org/) for more information.

## License

[The MIT License (MIT)](https://raw.githubusercontent.com/v2fly/v2ray-core/master/LICENSE)

## Credits

This repo relies on the following third-party projects:

- In production:
  - [gorilla/websocket](https://github.com/gorilla/websocket)
  - [gRPC](https://google.golang.org/grpc)
- For testing only:
  - [miekg/dns](https://github.com/miekg/dns)
  - [h12w/socks](https://github.com/h12w/socks)




客户端
在你的 PC （或手机）中，你需要运行 V2Ray 并使用下面的配置：

{
  "inbounds": [{
    "port": 1080,  // SOCKS 代理端口，在浏览器中需配置代理并指向这个端口
    "listen": "127.0.0.1",
    "protocol": "socks",
    "settings": {
      "udp": true
    }
  }],
  "outbounds": [{
    "protocol": "vmess",
    "settings": {
      "vnext": [{
        "address": "server", // 服务器地址，请修改为你自己的服务器 ip 或域名
        "port": 10086,  // 服务器端口
        "users": [{ "id": "b831381d-6324-4d53-ad4f-8cda48b30811" }]
      }]
    }
  },{
    "protocol": "freedom",
    "tag": "direct",
    "settings": {}
  }],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [{
      "type": "field",
      "ip": ["geoip:private"],
      "outboundTag": "direct"
    }]
  }
}
上述配置唯一要改的地方就是你的服务器 IP，配置中已注明。上述配置会把除了局域网（比如访问路由器）之外的所有流量转发到你的服务器。

服务器
然后你需要一台防火墙外的服务器，来运行服务器端的 V2Ray。配置如下：

{
  "inbounds": [{
    "port": 10086, // 服务器监听端口，必须和上面的一样
    "protocol": "vmess",
    "settings": {
      "clients": [{ "id": "b831381d-6324-4d53-ad4f-8cda48b30811" }]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
服务器的配置中需要确保 id 和端口与客户端一致，就可以正常连接了。

运行
在 Windows 和 macOS 中，配置文件通常是 V2Ray 同目录下的 config.json 文件。直接运行 v2ray 或 v2ray.exe 即可。
在 Linux 中，配置文件通常位于 /etc/v2ray/config.json 文件。运行 v2ray --config=/etc/v2ray/config.json，或使用 systemd 等工具把 V2Ray 作为服务在后台运行。
更多详见的说明可以参考白话文教程和配置文件说明。






