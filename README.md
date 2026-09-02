# Terraria IPv6 Relay

一个面向 Terraria TCP 联机的轻量 IPv6 中继。Ubuntu 服务端固定连接到指定的 Terraria 服务器，Windows 客户端在本机提供 `127.0.0.1:7777` 入口。

```text
Terraria 客户端 -> 127.0.0.1:7777 -> Windows Relay Client
               -> IPv6 Internet -> Ubuntu Relay Server -> Terraria Server:7777
```

## 快速开始

### 1. Ubuntu 启动中继服务端

将 Ubuntu 压缩包上传并解压，编辑 `terraria-relay.ini`：

```ini
[server]
listen_address = ::
listen_port = 17777
target_host = ::1
target_port = 7777
token = 请换成至少16位随机字符串
max_connections = 64
connect_timeout = 10
handshake_timeout = 10
keepalive = 15
```

然后运行：

```bash
chmod +x TerrariaRelay.Server
chmod 600 terraria-relay.ini
./TerrariaRelay.Server --config ./terraria-relay.ini
```


可选：作为 systemd 服务运行（压缩包内已附模板）：

```bash
#复制更新后的服务文件到系统目录
sudo cp /opt/net/terraria-relay.service /etc/systemd/system/
#给主程序执行权限
sudo chmod +x /opt/net/TerrariaRelay.Server
#重载systemd识别配置
sudo systemctl daemon-reload
#设置开机自启并立即启动
sudo systemctl enable --now terraria-relay
#查看状态
systemctl status terraria-relay

```

### 2. Windows 客户端

先完整解压 Windows 压缩包，再运行 `TerrariaRelay.Client.exe`。填写 Ubuntu 的公网 IPv6 或域名（IPv6 无需手动加 `[]`）、端口和同一个令牌，点击“启动中继”。客户端会先执行真实握手；成功后顶部显示“握手成功”，日志显示 Terraria 服务端可达。不要直接在压缩软件的预览窗口内运行 EXE。窗口标题应显示 `v1.1.0`。

然后在 Terraria 中选择“通过 IP 加入”，地址填写 `127.0.0.1`，端口填写 `7777`。

配置保存在 `%LOCALAPPDATA%\TerrariaIPv6Relay\config.json`，令牌使用 Windows DPAPI 加密后保存。

