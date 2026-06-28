# Mineradio Web Private

> 基于 [XxHuberrr/Mineradio](https://github.com/XxHuberrr/Mineradio) 二次适配的私人自用网页版。  
> 本版本保留原版 Mineradio 的沉浸式播放器、歌词舞台、粒子视觉、3D 歌单架和 DIY 体验，并增加网页部署、Cookie 登录、移动端触控和反向代理访问支持。

---

## 项目说明

Mineradio 原项目是一款以电影镜头、粒子视觉、歌词舞台和 3D 歌单架为核心的 Windows 桌面沉浸式音乐播放器。

本 Fork 版本主要将其适配为 **私人自用网页版**，方便部署到服务器后通过浏览器访问。

本项目仅供个人学习、研究和私人自用，不提供任何绕过音乐平台会员、付费内容、版权限制或重新分发音乐资源的能力。

---

## 本版本主要改动

- 移除 Electron 桌面壳依赖，支持 Node.js 服务端运行
- 支持通过浏览器访问播放器页面
- 增加私人访问密码保护
- 增加网易云音乐 / QQ 音乐 Cookie 导入登录模式
- 保留并适配 DIY 视觉控制台
- 增加手机 / 平板移动端触控支持
- 修复移动端 DIY 面板无法关闭的问题
- 支持 Apache / Nginx 反向代理部署
- 适合部署为私人网页播放器，例如 `https://radio.example.com`

---

## 功能特性

- 沉浸式音乐播放界面
- 歌词舞台
- 粒子视觉效果
- 3D 歌单架
- DIY 视觉控制台
- 网易云音乐 Cookie 登录
- QQ 音乐 Cookie 登录
- 手机 / 平板触控操作
- 私人访问密码
- 服务器部署访问
- Apache / Nginx 反向代理访问

---

## 环境要求

推荐环境：

```bash
Node.js 18+
```

如果是较老的 Windows Server，例如 Windows Server 2012 R2，建议使用：

```bash
Node.js 16.20.2
```

Windows Server 2019 / 2022 建议使用：

```bash
Node.js 20 LTS
```

---

## 安装与启动

克隆项目后进入项目目录：

```bash
npm install --omit=dev
npm start
```

默认访问地址：

```text
http://127.0.0.1:3000
```

如果部署在服务器上，可以通过：

```text
http://服务器IP:3000
```

或者通过 Apache / Nginx 反向代理到域名访问：

```text
https://radio.example.com
```

---

## 私人访问配置

项目根目录建议提供示例配置文件：

```text
private-web.config.example.json
```

首次使用时，复制一份为：

```text
private-web.config.json
```

示例内容：

```json
{
  "port": 3000,
  "password": "change-me"
}
```

请将 `password` 改成自己的访问密码。

请注意：

```text
private-web.config.json
```

不要上传到 GitHub。

---

## Cookie 登录说明

服务器网页版扫码登录可能受到网易云音乐 / QQ 音乐风控影响，因此本版本增加了手动 Cookie 导入模式。

网易云音乐 Cookie 通常需要包含：

```text
MUSIC_U=...
```

QQ 音乐 Cookie 通常需要包含：

```text
uin=...; qqmusic_key=...; qm_keyst=...
```

Cookie 属于敏感登录凭证，请勿分享、截图或上传到公开仓库。

本项目默认会将 Cookie 保存到本地文件：

```text
.cookie
.qq-cookie
```

这两个文件必须加入 `.gitignore`，不要提交到 GitHub。

---

## Cookie 获取方式

### 网易云音乐

1. 使用 Chrome / Edge 打开：

```text
https://music.163.com
```

2. 登录自己的网易云音乐账号。
3. 按 `F12` 打开开发者工具。
4. 打开 `Network / 网络`。
5. 刷新页面。
6. 点开任意 `music.163.com` 请求。
7. 在 `Headers / 标头` 中找到 `Request Headers`。
8. 复制 `Cookie:` 后面的一整串内容。
9. 粘贴到本项目的网易云 Cookie 导入框中。

注意：复制时不要带 `Cookie:` 这几个字，只复制后面的内容。

### QQ 音乐

1. 使用 Chrome / Edge 打开：

```text
https://y.qq.com
```

2. 登录自己的 QQ 音乐账号。
3. 按 `F12` 打开开发者工具。
4. 打开 `Network / 网络`。
5. 刷新页面。
6. 点开任意 `y.qq.com`、`c.y.qq.com` 或 `u.y.qq.com` 请求。
7. 在 `Headers / 标头` 中找到 `Request Headers`。
8. 复制 `Cookie:` 后面的一整串内容。
9. 粘贴到本项目的 QQ 音乐 Cookie 导入框中。

注意：Cookie 等同于登录凭证，请不要泄露。

---

## 移动端适配

本版本增加了手机和平板触控适配：

- 单指拖动
- 双指缩放
- 歌单架滑动
- DIY 面板关闭按钮
- 移动端点击面板外区域收起 DIY 面板

如果移动端仍有局部区域无法操作，可以继续针对具体模块适配。

---

## Apache 反向代理示例

如果 Node 服务运行在：

```text
http://127.0.0.1:3000
```

Apache 80 端口反代示例：

```apache
<VirtualHost *:80>
    ServerName radio.example.com

    ProxyPreserveHost On
    ProxyRequests Off

    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/

    ErrorLog "logs/radio-error.log"
    CustomLog "logs/radio-access.log" common
</VirtualHost>
```

需要确保 Apache 开启以下模块：

```apache
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule headers_module modules/mod_headers.so
LoadModule rewrite_module modules/mod_rewrite.so
```

如果使用 WebSocket，还需要开启：

```apache
LoadModule proxy_wstunnel_module modules/mod_proxy_wstunnel.so
```

---

## Nginx 反向代理示例

如果使用 Nginx，可以参考：

```nginx
server {
    listen 80;
    server_name radio.example.com;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

---

## 推荐部署方式

更推荐使用域名和 HTTPS 访问：

```text
https://radio.example.com
```

推荐结构：

```text
浏览器
  ↓
HTTPS / 443
  ↓
Apache 或 Nginx
  ↓
http://127.0.0.1:3000
  ↓
Mineradio Web Private
```

不建议长期直接暴露：

```text
http://服务器IP:3000
```

---

## 安全提醒

请不要提交以下文件到 GitHub：

```text
node_modules/
dist/
release/
*.log

.cookie
.qq-cookie
private-web.config.json

.env
.env.local

*.key
*.pem
*.pfx
*.crt
*.csr

uploads/
cache/
tmp/
```

尤其不要上传：

- 网易云 Cookie
- QQ 音乐 Cookie
- 私人访问密码
- SSL 私钥
- 服务器真实配置
- 个人账号信息

建议仓库中只保留：

```text
private-web.config.example.json
```

不要保留真实配置文件。

---

## 建议的 .gitignore

可以参考：

```gitignore
node_modules/
dist/
release/
*.log

.cookie
.qq-cookie
private-web.config.json

.env
.env.local

*.key
*.pem
*.pfx
*.crt
*.csr

uploads/
cache/
tmp/
```

---

## 注意事项

1. 本项目是原 Mineradio 的二次适配版本，不是官方原版。
2. 第三方音乐平台接口可能因平台规则变化而失效。
3. 服务器扫码登录可能出现风控，推荐使用 Cookie 导入模式。
4. Cookie 属于敏感登录凭证，请勿公开。
5. 本项目仅建议用于私人自用，不建议作为公开音乐站点运营。
6. 本项目不提供任何绕过会员、付费、版权或平台限制的能力。

---

## 免责声明

本项目仅供个人学习、研究和私人自用。

本项目不是网易云音乐、QQ 音乐或腾讯音乐娱乐集团的官方客户端，也不提供任何绕过付费、会员、版权或平台限制的能力。

第三方音乐平台接口、登录状态、Cookie、播放链接等可能因平台规则变化而失效。

使用者应自行遵守相关平台服务协议、版权规则和当地法律法规。

---

## 致谢

本项目基于原项目二次修改：

- 原项目：[XxHuberrr/Mineradio](https://github.com/XxHuberrr/Mineradio)
- 原作者：XxHuberrr

感谢原作者开源 Mineradio。

---

## License

本项目继承原项目授权，继续遵循 GPL-3.0 License。

请保留原项目的 `LICENSE`、`NOTICE` 及相关版权说明。
