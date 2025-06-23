# Clash Configuration

个人使用的 Clash 代理配置文件，支持链式代理、负载均衡和智能分流。

## 📋 目录

- [特性](#特性)
- [配置结构](#配置结构)
- [使用方法](#使用方法)
- [策略组说明](#策略组说明)
- [自定义规则](#自定义规则)
- [配置说明](#配置说明)
- [注意事项](#注意事项)

## ✨ 特性

- 🔗 **链式代理支持** - 支持前置节点 + 落地节点的链式代理配置
- ⚖️ **负载均衡** - 使用一致性哈希算法确保访问稳定性
- 🎯 **智能分流** - 基于域名和 IP 的精确分流规则
- 🛠️ **开发工具优化** - 针对 AI 编程工具的专门配置
- 🚫 **广告拦截** - 集成 AWAvenue 广告拦截规则
- 🌐 **全面覆盖** - 支持主流流媒体、社交媒体和开发平台

## 📁 配置结构

```
clash_conf/
├── clash.yaml          # 主配置文件
├── filter/             # 自定义规则目录
│   ├── cursor.list     # Cursor AI 编辑器规则
│   ├── windsurf.list   # Windsurf 编辑器规则
│   └── augment.list    # Augment Code 规则
└── README.md           # 说明文档
```

## 🚀 使用方法

### 1. 配置机场订阅

编辑 `clash.yaml` 文件，在 `proxy-providers` 部分填入你的机场订阅 URL：

```yaml
proxy-providers:
  Sub:
    url: "你的机场订阅URL"  # 替换为实际的订阅地址
```

### 2. 配置落地节点（可选）

如果需要使用链式代理，在 `proxies` 部分配置你的落地节点：

```yaml
proxies:
  - name: 落地节点_1
    type: socks5
    server: your_server_ip     # 替换为实际IP
    port: your_port           # 替换为实际端口
    username: your_username   # 替换为实际用户名
    password: your_password   # 替换为实际密码
```

### 3. 导入配置

将 `clash.yaml` 导入到你的 Clash 客户端中。

## 🎛️ 策略组说明

### 主要策略组

| 策略组 | 类型 | 说明 |
|--------|------|------|
| **节点选择** | `select` | 手动选择代理方式 |
| **链式代理落地** | `select` | 链式代理的落地节点组 |
| **链式代理前置** | `select` | 链式代理的前置节点组 |
| **手动选择** | `select` | 包含所有节点的手动选择组 |

### 地区策略组

| 地区 | 策略类型 | 算法 | 说明 |
|------|----------|------|------|
| **香港自动** | `load-balance` | `consistent-hashing` | 香港节点负载均衡 |
| **日本自动** | `load-balance` | `consistent-hashing` | 日本节点负载均衡 |
| **台湾自动** | `load-balance` | `consistent-hashing` | 台湾节点负载均衡 |
| **新加坡自动** | `load-balance` | `consistent-hashing` | 新加坡节点负载均衡 |
| **美国自动** | `url-test` | - | 美国节点自动选择最快 |
| **自动选择** | `url-test` | - | 全部节点自动选择最快 |

### 应用策略组

| 应用 | 默认策略 | 说明 |
|------|----------|------|
| **AI** | 链式代理落地 | AI 工具（OpenAI、Claude、Gemini 等） |
| **Google** | 节点选择 | Google 服务 |
| **YouTube** | 节点选择 | YouTube 视频服务 |
| **GitHub** | 节点选择 | GitHub 代码托管 |
| **Telegram** | 节点选择 | Telegram 即时通讯 |
| **Games** | 节点选择 | 游戏平台（Steam、Epic 等） |
| **Stream** | 节点选择 | 流媒体（Netflix、Disney+ 等） |

## 🛠️ 自定义规则

项目包含针对开发工具的自定义规则：

### AI 编程工具

- **Cursor**: `cursor.com`, `cursorapi.com`, `cursor-cdn.com`
- **Windsurf**: `windsurf.com`, `codeium.com`
- **Augment**: `augmentcode.com`

这些工具的流量会自动路由到 AI 策略组，确保稳定访问。

## ⚙️ 配置说明

### 负载均衡策略

使用 `consistent-hashing` 算法的优势：
- 🔒 **访问一致性** - 相同网站总是使用同一节点
- 🔄 **会话保持** - 避免频繁切换导致的登录问题
- ⚖️ **负载分散** - 不同网站分散到不同节点

### 健康检查

- **检查间隔**: 900秒（15分钟）
- **超时时间**: 5秒
- **容错次数**: 5次
- **检查URL**: `https://www.gstatic.com/generate_204`

### DNS 配置

- **模式**: `fake-ip` 模式，提升解析速度
- **国内DNS**: 223.5.5.5, 119.29.29.29
- **国外DNS**: DNS over HTTPS (Cloudflare, Google)
- **分流解析**: 国内域名使用国内DNS，国外域名使用国外DNS

## ⚠️ 注意事项

### 使用前准备

1. **机场订阅**: 确保有有效的机场订阅链接
2. **客户端版本**: 建议使用 Clash Meta 内核
3. **权限设置**: 确保 Clash 有网络访问权限

### 链式代理配置

如果不需要链式代理功能：
1. 可以直接使用其他策略组（如"香港自动"、"美国自动"等）
2. 删除或注释掉 `proxies` 部分的落地节点配置

### 规则更新

配置中的规则会自动从以下源更新：
- **广告拦截**: AWAvenue-Ads-Rule
- **GEO数据**: MetaCubeX/meta-rules-dat
- **应用规则**: blackmatrix7/ios_rule_script

### 性能优化

- 启用了 `unified-delay` 统一延迟测试
- 启用了 `tcp-concurrent` TCP 并发连接
- 配置了合适的 MTU 值（9000）

## 📝 更新日志

- **v1.0**: 初始版本，支持基础代理功能
- **v1.1**: 添加链式代理支持
- **v1.2**: 优化负载均衡策略，使用一致性哈希
- **v1.3**: 添加 AI 编程工具专用规则

## 📄 许可证

本项目仅供个人学习和使用，请遵守当地法律法规。

---

**⭐ 如果这个配置对你有帮助，欢迎 Star 支持！**
