# 配置说明

FileCodeBox 提供了丰富的配置选项，可以通过管理面板或直接修改配置来自定义系统行为。本文档详细介绍所有可用的配置项。

## 配置方式

FileCodeBox 支持两种配置方式：

1. **管理面板配置**（推荐）：访问 `/admin` 进入管理面板，在设置页面修改配置
2. **数据库配置**：配置存储在 `data/filecodebox.db` 数据库中

::: tip 提示
首次启动时，系统会使用 `core/settings.py` 中的默认配置。修改后的配置会保存到数据库中。
:::

## 基础设置

### 站点信息

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `name` | string | `文件快递柜 - FileCodeBox` | 站点名称，显示在页面标题和导航栏 |
| `description` | string | `开箱即用的文件快传系统` | 站点描述，用于 SEO |
| `keywords` | string | `FileCodeBox, 文件快递柜...` | 站点关键词，用于 SEO |
| `port` | int | `12345` | 服务监听端口 |

### 环境变量

| 环境变量 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `BASE_PATH` | string | `""` | 反向代理子路径，用于部署在子路径下（如 `/filebox`） |

::: tip 反向代理配置
如果需要将 FileCodeBox 部署在 Nginx 反向代理的子路径下（如 `https://example.com/filebox/`），请设置 `BASE_PATH` 环境变量。详见下方反向代理配置示例。
:::

### 通知设置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `notify_title` | string | `系统通知` | 通知标题 |
| `notify_content` | string | 欢迎信息 | 通知内容，支持 HTML |
| `page_explain` | string | 法律声明 | 页面底部说明文字 |
| `robotsText` | string | `User-agent: *\nDisallow: /` | robots.txt 内容 |

## 上传设置

### 文件上传限制

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `openUpload` | int | `1` | 是否开启上传功能（1=开启，0=关闭） |
| `uploadSize` | int | `10485760` | 单文件最大上传大小（字节），默认 10MB |
| `enableChunk` | int | `0` | 是否启用分片上传（1=启用，0=禁用） |

::: warning 注意
`uploadSize` 的单位是字节。10MB = 10 * 1024 * 1024 = 10485760 字节
:::

### 上传频率限制

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `uploadMinute` | int | `1` | 上传限制的时间窗口（分钟） |
| `uploadCount` | int | `10` | 在时间窗口内允许的最大上传次数 |

例如：默认配置表示每 1 分钟内最多允许上传 10 次。

### 文件过期设置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `expireStyle` | list | `["day","hour","minute","forever","count"]` | 可选的过期方式 |
| `max_save_seconds` | int | `0` | 文件最大保存时间（秒），0 表示不限制 |

过期方式说明：
- `day` - 按天过期
- `hour` - 按小时过期
- `minute` - 按分钟过期
- `forever` - 永不过期
- `count` - 按下载次数过期

## 主题设置

### 主题选择

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `themesSelect` | string | `themes/2024` | 当前使用的主题 |
| `themesChoices` | list | 见下方 | 可用主题列表 |

默认可用主题：
```json
[
  {
    "name": "2023",
    "key": "themes/2023",
    "author": "Lan",
    "version": "1.0"
  },
  {
    "name": "2024",
    "key": "themes/2024",
    "author": "Lan",
    "version": "1.0"
  }
]
```

### 界面样式

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `opacity` | float | `0.9` | 界面透明度（0-1） |
| `background` | string | `""` | 自定义背景图片 URL，为空则使用默认背景 |

## 管理员设置

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `admin_token` | string | `FileCodeBox2023` | 管理员登录密码 |
| `showAdminAddr` | int | `0` | 是否在首页显示管理入口（1=显示，0=隐藏） |

::: danger 安全警告
请务必在生产环境中修改默认的 `admin_token`！使用默认密码会导致严重的安全风险。
:::

## 安全设置

### 错误次数限制

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `errorMinute` | int | `1` | 错误限制的时间窗口（分钟） |
| `errorCount` | int | `1` | 在时间窗口内允许的最大错误次数 |

此设置用于防止暴力破解提取码。

## 存储设置

### 存储类型

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `file_storage` | string | `local` | 存储后端类型 |
| `storage_path` | string | `""` | 自定义存储路径 |

支持的存储类型：
- `local` - 本地存储
- `s3` - S3 兼容存储（AWS S3、阿里云 OSS、MinIO 等）
- `onedrive` - OneDrive 存储
- `webdav` - WebDAV 存储
- `opendal` - OpenDAL 存储

详细的存储配置请参考 [存储配置](/guide/storage)。

## 配置示例

### 示例 1：小型个人使用

适合个人或小团队使用，限制较宽松：

```python
{
    "name": "我的文件分享",
    "uploadSize": 52428800,        # 50MB
    "uploadMinute": 5,             # 5分钟
    "uploadCount": 20,             # 最多20次
    "expireStyle": ["day", "hour", "forever"],
    "admin_token": "your-secure-password",
    "showAdminAddr": 1
}
```

### 示例 2：公开服务

适合公开服务，需要更严格的限制：

```python
{
    "name": "公共文件快递柜",
    "uploadSize": 10485760,        # 10MB
    "uploadMinute": 1,             # 1分钟
    "uploadCount": 5,              # 最多5次
    "errorMinute": 5,              # 5分钟
    "errorCount": 3,               # 最多3次错误
    "expireStyle": ["hour", "minute", "count"],
    "max_save_seconds": 86400,     # 最长保存1天
    "admin_token": "very-secure-password-123",
    "showAdminAddr": 0
}
```

### 示例 3：企业内部使用

适合企业内部使用，支持大文件和分片上传：

```python
{
    "name": "企业文件中转站",
    "uploadSize": 1073741824,      # 1GB
    "enableChunk": 1,              # 启用分片上传
    "uploadMinute": 10,            # 10分钟
    "uploadCount": 100,            # 最多100次
    "expireStyle": ["day", "forever"],
    "file_storage": "s3",          # 使用S3存储
    "admin_token": "enterprise-secure-token",
    "showAdminAddr": 1
}
```

## 反向代理配置

### Nginx 配置

#### 部署在根路径

如果将 FileCodeBox 部署在根路径（如 `https://example.com/`），使用以下配置：

```nginx
location / {
    proxy_pass http://localhost:12345;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

#### 部署在子路径

如果需要将 FileCodeBox 部署在子路径（如 `https://example.com/filebox/`），需要配置环境变量和 Nginx：

**1. 启动 FileCodeBox 时设置环境变量：**

```bash
# 使用 Docker
docker run -d \
  -e BASE_PATH="/filebox" \
  -p 12345:12345 \
  -v /opt/FileCodeBox/:/app/data \
  --name filecodebox \
  lanol/filecodebox:latest

# 或手动运行
export BASE_PATH="/filebox"
python main.py
```

**2. 配置 Nginx：**

```nginx
location /filebox/ {
    proxy_pass http://localhost:12345/filebox/;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

::: warning 注意
- `BASE_PATH` 必须以 `/` 开头，如 `/filebox`
- Nginx 配置中的路径必须与 `BASE_PATH` 一致
- `proxy_pass` 中的 URL 必须包含子路径（如 `http://localhost:12345/filebox/`）
:::

### Docker Compose 配置

在 `docker-compose.yml` 中配置子路径：

```yaml
version: "3"
services:
  file-code-box:
    image: lanol/filecodebox:latest
    environment:
      - BASE_PATH=/filebox
    volumes:
      - fcb-data:/app/data:rw
    restart: unless-stopped
    ports:
      - "12345:12345"

volumes:
  fcb-data:
    external: false
```

## 下一步

- [存储配置](/guide/storage) - 了解如何配置不同的存储后端
- [安全设置](/guide/security) - 了解如何增强系统安全性
- [文件分享](/guide/share) - 了解文件分享功能
