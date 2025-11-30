# Sora2API

一个功能完整的 OpenAI 兼容 API 服务，为 Sora 提供统一的接口。

## 快速开始

### Docker 部署（推荐）

```bash
# 克隆项目
git clone https://github.com/GakkiNoOne/sora2api.git
cd sora2api

# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f
```

### WARP 模式（使用代理）

```bash
docker-compose -f docker-compose.warp.yml up -d
```

### 首次访问

- **地址**: http://localhost:8000
- **用户名**: `admin`
- **密码**: `admin`

⚠️ **首次登录后请立即修改密码！**

## 配置说明

配置文件位于 `config/setting.toml`，所有配置项如下：

### [global] - 全局配置

```toml
[global]
api_key = "han1234"              # API 访问密钥
admin_username = "admin"          # 管理员用户名
admin_password = "admin"          # 管理员密码
```

### [sora] - Sora API 配置

```toml
[sora]
base_url = "https://sora.chatgpt.com/backend"  # Sora API 基础地址
timeout = 120                                   # 请求超时时间（秒）
max_retries = 3                                 # 最大重试次数
poll_interval = 2.5                             # 轮询间隔（秒）
max_poll_attempts = 600                         # 最大轮询次数
```

### [server] - 服务器配置

```toml
[server]
host = "0.0.0.0"    # 监听地址
port = 8000         # 监听端口
```

### [debug] - 调试配置

```toml
[debug]
enabled = false          # 是否启用调试模式
log_requests = true      # 是否记录请求日志
log_responses = true     # 是否记录响应日志
mask_token = true        # 是否在日志中隐藏 token
```

### [cache] - 缓存配置

```toml
[cache]
enabled = false                      # 是否启用缓存
timeout = 600                        # 缓存超时时间（秒）
base_url = "http://127.0.0.1:8000"  # 缓存基础 URL
```

### [generation] - 生成配置

```toml
[generation]
image_timeout = 300     # 图片生成超时时间（秒）
video_timeout = 1500    # 视频生成超时时间（秒）
```

### [admin] - 管理配置

```toml
[admin]
error_ban_threshold = 3  # 错误达到此次数后禁用 token
```

### [proxy] - 代理配置

```toml
[proxy]
proxy_enabled = false    # 是否启用代理
proxy_url = ""          # 代理地址（如：socks5://127.0.0.1:1080）
```

### [watermark_free] - 无水印配置

```toml
[watermark_free]
watermark_free_enabled = false         # 是否启用无水印模式
parse_method = "third_party"           # 解析方法（third_party/custom）
custom_parse_url = ""                  # 自定义解析服务 URL
custom_parse_token = ""                # 自定义解析服务 Token
```

### [token_refresh] - Token 刷新配置

```toml
[token_refresh]
at_auto_refresh_enabled = false                                                              # 是否启用 AT 自动刷新
client_ids = "app_LlGpXReQgckcGGUo2JrYvtJK,app_WXrF1LSkiTtfYqiL6XtjygvX"  # 🆕 Client ID 列表（逗号分隔）
```

**🆕 新增配置项说明：**

- `client_ids`: 用于 RT 刷新 AT 的 Client ID 列表，支持多个用逗号分隔
  - 系统会依次尝试每个 client_id 进行刷新
  - 刷新成功后会自动记录并绑定该 token 使用的 client_id
  - 后续刷新优先使用已绑定的 client_id

## API 使用

### 端点

```
POST http://localhost:8000/v1/chat/completions
```

### 认证

```bash
Authorization: Bearer YOUR_API_KEY
```

### 支持的模型

**图片模型**
- `sora-image` - 文生图（360×360）
- `sora-image-landscape` - 文生图横屏（540×360）
- `sora-image-portrait` - 文生图竖屏（360×540）

**视频模型**
- `sora-video-10s` / `sora-video-15s` - 横屏视频
- `sora-video-landscape-10s` / `sora-video-landscape-15s` - 横屏视频
- `sora-video-portrait-10s` / `sora-video-portrait-15s` - 竖屏视频

### 示例

**文生图**

```bash
curl -X POST "http://localhost:8000/v1/chat/completions" \
  -H "Authorization: Bearer han1234" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "sora-image",
    "messages": [{"role": "user", "content": "一只可爱的小猫"}],
    "stream": true
  }'
```

**文生视频**

```bash
curl -X POST "http://localhost:8000/v1/chat/completions" \
  -H "Authorization: Bearer han1234" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "sora-video-landscape-10s",
    "messages": [{"role": "user", "content": "一只小猫在草地上奔跑"}],
    "stream": true
  }'
```

## 许可证

MIT License

## 致谢

感谢所有贡献者和使用者的支持！

---

⭐ 如果这个项目对你有帮助，请给个 Star！
