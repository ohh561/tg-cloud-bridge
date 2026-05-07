# 🤖 TG Super Bot

Telegram 文件下载到网盘机器人。将 Telegram 文件自动上传到 AList/OpenList 支持的各种云存储（OneDrive、Google Drive 等），支持生成公网下载链接。

## ✨ 功能

- **双模式上传** — 🔒混淆（Crypt 文件名加密）/ 📤直传（OneDrive 原始文件名）
- **按钮选择** — 发送文件后直接弹出模式选择按钮
- **🔗 下载链接** — 上传完成后一键生成公网分享链接
- **并发传输** — 同时处理多个文件（默认 3 个）
- **自动分类** — 按文件类型（视频/音频/图片/文件）自动归类
- **进度显示** — 下载和上传双进度条
- **失败重试** — 上传失败保留文件，点击按钮重试
- **文件夹选择** — `/folder` 浏览 AList 目录，设置默认上传路径
- **取消上传** — `/cancel` 取消正在进行的上传
- **自动删除** — `/autodel` 上传成功后自动删除原始消息
- **自动清理** — 定时清理临时文件和过期任务

## 📦 依赖

- [Telegram Bot API Server](https://github.com/tdlib/telegram-bot-api) — 本地部署，支持最大 2GB 文件
- [OpenList](https://github.com/OpenListTeam/OpenList) / [Alist](https://github.com/alist-org/alist) — WebDAV 网盘管理

## 🚀 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/ohh561/tg-super-bot.git
cd tg-super-bot
```

### 2. 配置环境变量

```bash
cp .env.example .env
vim .env
```

### 3. 启动服务

```bash
docker compose up -d
```

## ⚙️ 配置说明

| 变量 | 说明 | 示例 |
|------|------|------|
| `BOT_TOKEN` | Telegram Bot Token | `123456:ABC-DEF...` |
| `ALLOWED_USER_ID` | 允许使用的用户 ID | `123456789` |
| `ALIST_WEBDAV_CRYPT` | 混淆模式 WebDAV 地址 | `http://openlist:5244/dav/PrivateVideo/` |
| `ALIST_WEBDAV_DIRECT` | 直传模式 WebDAV 地址 | `http://openlist:5244/dav/Onedrive/` |
| `ALIST_API_URL` | AList API 地址（用于文件夹浏览和分享） | `http://openlist:5244` |
| `ALIST_USER` | Alist/OpenList 用户名 | `admin` |
| `ALIST_PASS` | Alist/OpenList 密码 | `password` |
| `PUBLIC_URL` | AList 公网地址（用于生成下载链接） | `https://openlist.example.com` |

> 💡 `PUBLIC_URL` 为可选配置。设置后，上传完成会显示「获取下载链接」按钮，点击即可生成公网可访问的分享链接。

## 📁 目录结构

**混淆模式（Crypt）：**
```
PrivateVideo/
├── 视频/
├── 音频/
├── 图片/
└── 文件/
```

**直传模式（OneDrive）：**
```
Onedrive/telegram/
├── 视频/
├── 音频/
├── 图片/
└── 文件/
```

> 💡 使用 `/folder` 命令可以自定义直传模式的目标文件夹

## 🎮 命令

| 命令 | 说明 |
|------|------|
| `/start` | 显示帮助 |
| `/folder` | 选择上传文件夹（浏览 AList 目录） |
| `/autodel` | 开关自动删除消息 |
| `/cancel` | 取消当前上传 |
| `/status` | 检查连接状态 |
| `/retry` | 查看待重试文件 |

## 🔗 下载链接功能

配置 `PUBLIC_URL` 后，上传完成会显示「🔗 获取下载链接」按钮：

1. 发送文件到 Bot
2. 选择上传模式（混淆/直传）
3. 等待上传完成
4. 点击「🔗 获取下载链接」按钮
5. Bot 通过 AList API 创建分享链接并返回公网 URL

分享链接特点：
- 永久有效（除非手动删除）
- 无需密码（可配置）
- 直接下载，无需登录

## 🔧 自定义

修改 `bot.py` 中的配置：

```python
MAX_RETRIES = 3              # 重试次数
CONCURRENT_UPLOADS = 3       # 并发上传数
```

## 📄 License

MIT
