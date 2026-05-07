# 🌉 TG Cloud Bridge

Telegram 文件自动转存到云存储的机器人。通过 AList/OpenList 将 Telegram 文件上传到 OneDrive、Google Drive 等网盘，支持生成公网下载链接。

A Telegram bot that automatically transfers files to cloud storage via AList/OpenList. Supports OneDrive, Google Drive, and more — with public share link generation.

## ✨ 功能 / Features

- **双模式上传 / Dual Upload Modes** — 🔒混淆（Crypt 文件名加密）/ 📤直传（OneDrive 原始文件名）
- **按钮选择 / Inline Buttons** — 发送文件后弹出模式选择按钮
- **🔗 下载链接 / Share Links** — 上传完成后一键生成公网分享链接
- **并发传输 / Concurrent Uploads** — 同时处理多个文件（默认 3 个）
- **自动分类 / Auto Categorize** — 按文件类型（视频/音频/图片/文件）自动归类
- **进度显示 / Progress Bar** — 下载和上传双进度条
- **失败重试 / Retry on Failure** — 上传失败保留文件，点击按钮重试
- **文件夹选择 / Folder Browse** — `/folder` 浏览 AList 目录，设置默认上传路径
- **取消上传 / Cancel Upload** — `/cancel` 取消正在进行的上传
- **自动删除 / Auto Delete** — `/autodel` 上传成功后自动删除原始消息
- **自动清理 / Auto Cleanup** — 定时清理临时文件和过期任务

## 🚀 快速开始 / Quick Start

### 1. 创建 Telegram Bot / Create Telegram Bot

1. 在 Telegram 中找到 [@BotFather](https://t.me/BotFather)，发送 `/newbot`
2. 按提示设置 bot 名称和用户名
3. 获取 **Bot Token**（格式如 `123456:ABC-DEF...`）

> Find [@BotFather](https://t.me/BotFather) on Telegram, send `/newbot`, follow the prompts, and save the **Bot Token**.

### 2. 获取你的 Telegram User ID / Get Your User ID

在 Telegram 中找到 [@userinfobot](https://t.me/userinfobot)，发送任意消息，它会回复你的 **User ID**。

> Find [@userinfobot](https://t.me/userinfobot), send any message, and it replies with your **User ID**.

### 3. 获取 Telegram API 凭据 / Get Telegram API Credentials

本地 Bot API Server 需要 API ID 和 API Hash：

1. 访问 [my.telegram.org](https://my.telegram.org)
2. 用手机号登录
3. 进入 **API development tools**
4. 创建应用，获取 **App api_id** 和 **App api_hash**

> Visit [my.telegram.org](https://my.telegram.org), log in, go to **API development tools**, create an app, and get **api_id** and **api_hash**.

### 4. 准备 AList/OpenList / Set Up AList/OpenList

你需要一个运行中的 AList 或 OpenList 实例，并配置好存储驱动（如 OneDrive）。

You need a running AList/OpenList instance with a storage driver configured (e.g., OneDrive).

- [OpenList 安装指南 / Install Guide](https://github.com/OpenListTeam/OpenList#readme)
- [AList 安装指南 / Install Guide](https://github.com/alist-org/alist#readme)

记下 AList 的以下信息：
- API 地址（如 `http://localhost:5244`）
- WebDAV 地址（如 `http://localhost:5244/dav/Onedrive/`）
- 用户名和密码

### 5. 克隆项目 / Clone

```bash
git clone https://github.com/ohh561/tg-cloud-bridge.git
cd tg-cloud-bridge
```

### 6. 配置环境变量 / Configure

```bash
cp .env.example .env
vim .env
```

填写以下内容 / Fill in the following:

```env
# Telegram Bot
BOT_TOKEN=123456:ABC-DEF...          # 步骤 1 获取 / From step 1
ALLOWED_USER_ID=123456789            # 步骤 2 获取 / From step 2
TELEGRAM_API_ID=12345                # 步骤 3 获取 / From step 3
TELEGRAM_API_HASH=abcdef1234567890   # 步骤 3 获取 / From step 3

# AList / OpenList（步骤 4 获取 / From step 4）
ALIST_WEBDAV_CRYPT=http://openlist:5244/dav/PrivateVideo/
ALIST_WEBDAV_DIRECT=http://openlist:5244/dav/Onedrive/
ALIST_API_URL=http://openlist:5244
ALIST_USER=admin
ALIST_PASS=your_password

# 可选：公网地址（用于生成下载链接）
# Optional: public URL for share link generation
# PUBLIC_URL=https://openlist.example.com
```

### 7. 启动服务 / Start

```bash
docker compose up -d
```

### 8. 验证 / Verify

向你的 Bot 发送任意文件，如果弹出上传模式按钮，说明部署成功。

Send any file to your bot. If upload mode buttons appear, deployment is successful.

## ⚙️ 配置说明 / Configuration Reference

| 变量 / Variable | 必填 / Required | 说明 / Description | 示例 / Example |
|------|------|------|------|
| `BOT_TOKEN` | ✅ | Telegram Bot Token | `123456:ABC-DEF...` |
| `ALLOWED_USER_ID` | ✅ | 允许使用的用户 ID / Allowed user ID | `123456789` |
| `TELEGRAM_API_ID` | ✅ | Telegram API ID（本地 Bot API 用） | `12345` |
| `TELEGRAM_API_HASH` | ✅ | Telegram API Hash（本地 Bot API 用） | `abcdef1234567890` |
| `ALIST_WEBDAV_CRYPT` | ✅ | 混淆模式 WebDAV 地址 / Crypt mode WebDAV URL | `http://openlist:5244/dav/PrivateVideo/` |
| `ALIST_WEBDAV_DIRECT` | ✅ | 直传模式 WebDAV 地址 / Direct mode WebDAV URL | `http://openlist:5244/dav/Onedrive/` |
| `ALIST_API_URL` | ✅ | AList API 地址 / AList API endpoint | `http://openlist:5244` |
| `ALIST_USER` | ✅ | 用户名 / Username | `admin` |
| `ALIST_PASS` | ✅ | 密码 / Password | `password` |
| `PUBLIC_URL` | ❌ | 公网地址（生成下载链接用）/ Public URL for share links | `https://openlist.example.com` |

> 💡 `PUBLIC_URL` 为可选配置。设置后，上传完成会显示「获取下载链接」按钮。
>
> `PUBLIC_URL` is optional. When set, a "Get Download Link" button appears after upload.

## 📁 目录结构 / Directory Structure

**混淆模式 / Crypt Mode：**
```
PrivateVideo/
├── 视频/ (Videos)
├── 音频/ (Audio)
├── 图片/ (Images)
└── 文件/ (Files)
```

**直传模式 / Direct Mode：**
```
Onedrive/telegram/
├── 视频/ (Videos)
├── 音频/ (Audio)
├── 图片/ (Images)
└── 文件/ (Files)
```

> 💡 使用 `/folder` 命令可以自定义直传模式的目标文件夹
>
> Use `/folder` to customize the upload destination for direct mode

## 🎮 命令 / Commands

| 命令 / Command | 说明 / Description |
|------|------|
| `/start` | 显示帮助 / Show help |
| `/folder` | 选择上传文件夹 / Browse & select upload folder |
| `/autodel` | 开关自动删除 / Toggle auto-delete messages |
| `/cancel` | 取消当前上传 / Cancel ongoing uploads |
| `/status` | 检查连接状态 / Check connection status |
| `/retry` | 查看待重试文件 / List files pending retry |

## 🔗 下载链接 / Share Links

配置 `PUBLIC_URL` 后，上传完成会显示「🔗 获取下载链接」按钮：

With `PUBLIC_URL` configured, a share link button appears after upload:

1. 发送文件到 Bot / Send file to bot
2. 选择上传模式 / Choose upload mode
3. 等待上传完成 / Wait for upload
4. 点击「🔗 获取下载链接」/ Click share link button
5. 获取公网 URL / Get public download URL

分享链接特点 / Share link features:
- 永久有效 / Permanent (unless manually deleted)
- 无需密码 / No password required
- 直接下载 / Direct download, no login needed

## 🔧 自定义 / Customization

```python
MAX_RETRIES = 3              # 重试次数 / Retry attempts
CONCURRENT_UPLOADS = 3       # 并发上传数 / Concurrent uploads
```

## 🏗️ 架构 / Architecture

```
User → Telegram → telegram-bot-api (local, --local mode, 2GB limit)
                        ↓
                  File saved to disk
                        ↓
                  tg-cloud-bridge reads file
                        ↓
                  Uploads to AList/OpenList (WebDAV)
                        ↓
                  Cloud Storage (OneDrive / GDrive / etc.)
```

## 📄 License

MIT
