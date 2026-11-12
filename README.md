# Caddy 自动构建镜像（内置 Cloudflare DNS 模块）
使用 GitHub Actions 自动编译带 `cloudflare` DNS 插件的 Caddy 多架构镜像（amd64 / armv7 / arm64），支持定时自动构建、手动触发构建。
> 适合 Fork 后自己使用，构建产物推送到自己的 DockerHub。

## ✨ 特性
- ✅ 内置官方 `github.com/caddy-dns/cloudflare` 模块，支持 DNS-01 ACME 证书签发
- ✅ 多架构：`linux/amd64`、`linux/arm/v7`、`linux/arm64`（适配 x86、树莓派、ARM 服务器）
- ✅ GitHub Actions 定时构建：**北京时间每周一凌晨 02:00**
- ✅ 支持手动触发构建
- ✅ 镜像标签：`latest` + 构建日期标签（如 `2026-11-12`）
- ✅ 构建缓存，加快重复编译速度
- ✅ 敏感信息全部放在仓库 Secrets，公开仓库安全可用

---

## 📋 Fork 之后必须配置（重点）
Fork 本仓库到自己账号后，**先配置下面 3 个仓库 Secrets，否则 Action 无法推送镜像**
路径：仓库 → Settings → Secrets and variables → Actions → New repository secret

| Secret 名称 | 填写内容示例 | 说明 |
| ---- | ---- | ---- |
| `DOCKERHUB_USERNAME` | `your_dockerhub_name` | DockerHub 用户名 |
| `DOCKERHUB_TOKEN` | `dkp_xxxxxxxxxxxx` | DockerHub Access Token（推荐只给读写权限，不要用账号密码） |
| `DOCKER_IMAGE_NAME` | `yourname/caddy` | 你的镜像全名，例如 `john/caddy` |

> ⚠️ 提示：日志里镜像名称会显示为 `***`，这是 GitHub 自动掩码保护，不是故障。

### 如何获取 DockerHub Token
1. 登录 DockerHub → Account Settings → Personal access tokens
2. 新建 Token，权限勾选 `read, write`
3. 复制生成的 token，填入 `DOCKERHUB_TOKEN`

---

## 🕒 定时规则
- Cron 表达式：`0 18 * * 0`（UTC 时间）
- 对应：**北京时间每周一 02:00**
- GitHub Cron 存在 ±15 分钟浮动，属于正常现象

> 如果需要修改定时时间：注意 cron 是 UTC 时区，北京时间 = UTC+8

---

## 📦 镜像使用
### 拉取镜像
```bash
# 替换为你自己的镜像名
docker pull yourname/caddy:latest
# 或者日期版本
docker pull yourname/caddy:2026-11-12
