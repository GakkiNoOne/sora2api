# Sora2API

基于 [TheSmallHanCat/sora2api](https://github.com/TheSmallHanCat/sora2api) 的改进版本。

## 主要改动

🆕 **支持多 Client ID 刷新机制**
- 配置多个 client_id（逗号分隔），自动依次尝试
- 刷新成功后自动记录并绑定 client_id
- 后续刷新优先使用已绑定的 client_id

📦 **镜像更新**
- Docker 镜像：`ghcr.io/gakkinoone/sora2api:latest`

---

## 快速开始

```bash
git clone https://github.com/GakkiNoOne/sora2api.git
cd sora2api
docker-compose up -d
```

**首次访问**: http://localhost:8000 (admin/admin)

---

## 完整配置说明

配置文件：`config/setting.toml`

| 配置分组 | 配置项 | 默认值 | 说明 |
|---------|--------|--------|------|
| **[global]** | `api_key` | `"han1234"` | API 访问密钥 |
| | `admin_username` | `"admin"` | 管理员用户名 |
| | `admin_password` | `"admin"` | 管理员密码 |
| **[sora]** | `base_url` | `"https://sora.chatgpt.com/backend"` | Sora API 基础地址 |
| | `timeout` | `120` | 请求超时时间（秒） |
| | `max_retries` | `3` | 最大重试次数 |
| | `poll_interval` | `2.5` | 轮询间隔（秒） |
| | `max_poll_attempts` | `600` | 最大轮询次数 |
| **[server]** | `host` | `"0.0.0.0"` | 监听地址 |
| | `port` | `8000` | 监听端口 |
| **[debug]** | `enabled` | `false` | 是否启用调试模式 |
| | `log_requests` | `true` | 是否记录请求日志 |
| | `log_responses` | `true` | 是否记录响应日志 |
| | `mask_token` | `true` | 是否在日志中隐藏 token |
| **[cache]** | `enabled` | `false` | 是否启用缓存 |
| | `timeout` | `600` | 缓存超时时间（秒） |
| | `base_url` | `"http://127.0.0.1:8000"` | 缓存基础 URL |
| **[generation]** | `image_timeout` | `300` | 图片生成超时时间（秒） |
| | `video_timeout` | `1500` | 视频生成超时时间（秒） |
| **[admin]** | `error_ban_threshold` | `3` | 错误达到此次数后禁用 token |
| **[proxy]** | `proxy_enabled` | `false` | 是否启用代理 |
| | `proxy_url` | `""` | 代理地址（如：socks5://127.0.0.1:1080） |
| **[watermark_free]** | `watermark_free_enabled` | `false` | 是否启用无水印模式 |
| | `parse_method` | `"third_party"` | 解析方法（third_party/custom） |
| | `custom_parse_url` | `""` | 自定义解析服务 URL |
| | `custom_parse_token` | `""` | 自定义解析服务 Token |
| **[token_refresh]** | `at_auto_refresh_enabled` | `false` | 是否启用 AT 自动刷新 |
| | `client_ids` 🆕 | `"app_LlGpXReQgckcGGUo2JrYvtJK,app_WXrF1LSkiTtfYqiL6XtjygvX"` | Client ID 列表（逗号分隔） |

### 🆕 新增配置项详细说明

**client_ids** - Client ID 列表
- **格式**: 多个 client_id 用逗号分隔
- **用途**: 用于 RT（Refresh Token）刷新 AT（Access Token）
- **工作机制**:
  1. 依次尝试列表中的每个 client_id
  2. 刷新成功后自动记录到数据库
  3. 后续优先使用已记录的 client_id
  4. 每个 token 自动绑定最适合的 client_id

---

## API 使用说明

详细文档请参考原项目：[TheSmallHanCat/sora2api](https://github.com/TheSmallHanCat/sora2api)

**快速参考**:
- 端点: `POST http://localhost:8000/v1/chat/completions`
- 认证: `Authorization: Bearer YOUR_API_KEY`
- 格式: OpenAI 兼容格式

---

## 许可证

MIT License

---

⭐ 如果这个项目对你有帮助，请给个 Star！
