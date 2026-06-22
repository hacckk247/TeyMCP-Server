# 🚀 TeyMCP-Server

**The One MCP to Rule Them All** - 统一管理和调用所有MCP服务器的终极聚合器

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python  3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115+-green.svg)](https://fastapi.tiangolo.com)
[![MCP](https://img.shields.io/badge/MCP-1.1+-purple.svg)](https://modelcontextprotocol.io)

---

## 📖 简介

TeyMCP-Server 是一个强大的 MCP (Model Context Protocol) 聚合器，让你可以通过统一的接口管理和调用多个上游 MCP 服务器。

### ✨ 核心特性

- 🎯 **统一管理** - 一个接口调用所有MCP工具
- 🔄 **动态配置** - 热加载服务器，无需重启
- 🌐 **Web管理面板** - 直观的可视化管理界面
- 📊 **实时监控** - 服务器状态、调用日志、性能指标
- 🔧 **RESTful API** - 完整的HTTP API支持
- 🔌 **WebSocket** - 实时数据推送
- 🐳 **Docker支持** - 一键容器化部署
- ☸️ **Kubernetes就绪** - 生产级编排配置
- 📚 **完整文档** - 详尽的中文文档

---

## 🎬 快速演示

```bash
# 一键安装
curl -fsSL https://raw.githubusercontent.com/zf13883922290/TeyMCP-Server/main/scripts/install.sh | bash

# 配置
cp config/.env.example config/.env
nano config/.env

# 启动
bash scripts/start.sh

# 访问
open http://localhost:8080
```

---

## 📋 目录

- [安装](#-安装)
- [配置](#-配置)
- [使用](#-使用)
- [API文档](#-api文档)
- [部署](#-部署)
- [开发](#-开发)
- [贡献](#-贡献)
- [许可证](#-许可证)

---

## 🔧 安装

### 系统要求

- Python 3.11+
- Node.js 18+ (用于上游MCP)
- 512MB+ 内存
- Ubuntu 20.04+ / macOS / Windows WSL

### 快速安装

```bash
# 克隆仓库
git clone https://github.com/zf13883922290/TeyMCP-Server.git
cd TeyMCP-Server

# 运行安装脚本
bash scripts/install.sh

# 配置环境变量
cp config/.env.example config/.env
nano config/.env

# 启动服务
bash scripts/start.sh
```

### Docker安装

```bash
# 使用Docker Compose
cd docker
docker-compose up -d

# 查看日志
docker-compose logs -f
```

详细安装说明: [📖 安装文档](docs/QUICKSTART.md)

---

## ⚙️ 配置

### 环境变量 (config/.env)

```env
# GitHub Token
GITHUB_TOKEN=ghp_your_github_token_here

# Gitee Token
GITEE_TOKEN=your_gitee_token_here

# 应用配置
APP_HOST=0.0.0.0
APP_PORT=8080
LOG_LEVEL=INFO
```

### 服务器配置 (config/servers.yaml)

```yaml
servers:
  github:
    command: npx
    args:
      - "-y"
      - "@modelcontextprotocol/server-github"
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: ${GITHUB_TOKEN}
    enabled: true
    critical: true
    description: "GitHub官方MCP"
  
  gitee:
    command: npx
    args:
      - "-y"
      - "@gitee/mcp-gitee@latest"
    env:
      GITEE_ACCESS_TOKEN: ${GITEE_TOKEN}
    enabled: true
    critical: true
    description: "Gitee官方MCP"
```

完整配置说明: [📖 配置文档](docs/CONFIGURATION.md)

---

## 🚀 使用

### Web管理面板

访问 http://localhost:8080 查看：

- 📊 服务器状态监控
- 🔧 可用工具列表
- 📝 实时调用日志
- 📈 性能指标统计

### API调用示例

#### 查看所有工具

```bash
curl http://localhost:8080/api/tools
```

#### 调用工具

```bash
curl -X POST http://localhost:8080/api/tools/github_create_issue/call \
  -H "Content-Type: application/json" \
  -d '{
    "arguments": {
      "owner": "zf13883922290",
      "repo": "TeyMCP-Server",
      "title": "Test Issue",
      "body": "Created via API"
    }
  }'
```

#### 查看服务器状态

```bash
curl http://localhost:8080/api/servers
```

完整API文档: [📖 API文档](docs/API.md)

---

## 📡 API文档

TeyMCP-Server 提供完整的 RESTful API：

- 📊 **状态查询** - `/api/status`, `/api/health`
- 🖥️ **服务器管理** - `/api/servers/*`
- 🔧 **工具管理** - `/api/tools/*`
- 📝 **日志查询** - `/api/logs`
- 🔌 **WebSocket** - `/ws`

交互式API文档: http://localhost:8080/api/docs

---

## 🚢 部署

### 单机部署

```bash
# 使用systemd
sudo cp scripts/teymcp.service /etc/systemd/system/
sudo systemctl enable teymcp
sudo systemctl start teymcp
```

### Docker部署

```bash
# 构建镜像
docker build -t teymcp-server -f docker/Dockerfile .

# 运行容器
docker run -d \
  --name teymcp-server \
  -p 8080:8080 \
  -v $(pwd)/config:/app/config \
  teymcp-server
```

### Kubernetes部署

```bash
# 应用配置
kubectl apply -f k8s/

# 查看状态
kubectl get pods -n teymcp
```

详细部署指南: [📖 部署文档](docs/DEPLOYMENT.md)

---

## 💻 开发

### 项目结构

```
TeyMCP-Server/
├── src/
│   ├── main.py              # 主入口
│   ├── core/                # 核心逻辑
│   │   └── aggregator.py    # MCP聚合器
│   ├── api/                 # API路由
│   ├── web/                 # Web界面
│   ├── models/              # 数据模型
│   └── utils/               # 工具函数
├── config/                  # 配置文件
├── docs/                    # 文档
├── scripts/                 # 脚本
├── docker/                  # Docker配置
└── tests/                   # 测试
```

### 本地开发

```bash
# 安装开发依赖
pip install -r requirements-dev.txt

# 运行测试
pytest

# 代码格式化
black src/
isort src/

# 启动开发服务器
uvicorn src.main:app --reload
```

### 添加新的MCP服务器

1. 编辑 `config/servers.yaml`
2. 添加服务器配置
3. 重启服务或调用 `/api/servers` 添加

开发文档: [📖 开发指南](docs/DEVELOPMENT.md)

---

## 🤝 贡献

欢迎贡献代码、报告问题或提出建议！

### 如何贡献

1. Fork 仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

贡献指南: [📖 CONTRIBUTING.md](CONTRIBUTING.md)

---

## 📚 文档

- [📖 快速入门](docs/QUICKSTART.md)
- [⚙️ 配置说明](docs/CONFIGURATION.md)
- [📡 API文档](docs/API.md)
- [🚀 部署指南](docs/DEPLOYMENT.md)
- [🐳 Docker部署](docs/DOCKER.md)
- [☸️ Kubernetes部署](docs/KUBERNETES.md)
- [❓ 常见问题](docs/FAQ.md)
- [🔍 故障排查](docs/TROUBLESHOOTING.md)
- [📝 变更日志](docs/CHANGELOG.md)

---

## 🌟 支持的MCP服务器

- ✅ GitHub - 官方GitHub MCP
- ✅ Gitee - Gitee中国版
- ✅ Filesystem - 文件系统访问
- ✅ PostgreSQL - 数据库操作
- ✅ SQLite - 轻量数据库
- ✅ Slack - 团队协作
- ✅ AWS - 云服务管理
- ✅ Cloudflare - CDN和边缘计算
- 🔄 更多支持中...

查看完整列表: [支持的MCP服务器](docs/SUPPORTED_SERVERS.md)

---

## 📊 统计

![GitHub stars](https://img.shields.io/github/stars/zf13883922290/TeyMCP-Server?style=social)
![GitHub forks](https://img.shields.io/github/forks/zf13883922290/TeyMCP-Server?style=social)
![GitHub issues](https://img.shields.io/github/issues/zf13883922290/TeyMCP-Server)
![GitHub pull requests](https://img.shields.io/github/issues-pr/zf13883922290/TeyMCP-Server)

---

## 🆘 获取帮助

- 📧 Email: support@example.com
- 💬 Discord: [加入社区](https://discord.gg/xxx)
- 🐛 GitHub Issues: [提交问题](https://github.com/zf13883922290/TeyMCP-Server/issues)
- 💡 Discussions: [参与讨论](https://github.com/zf13883922290/TeyMCP-Server/discussions)

---

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

---

## 🙏 致谢

感谢所有为项目做出贡献的开发者！

特别感谢：
- [Anthropic](https://www.anthropic.com/) - MCP协议开发者
- [FastAPI](https://fastapi.tiangolo.com/) - 优秀的Web框架
- 所有MCP服务器开发者

---

## ⭐ Star历史

[![Star History Chart](https://api.star-history.com/svg?repos=zf13883922290/TeyMCP-Server&type=Date)](https://star-history.com/#zf13883922290/TeyMCP-Server&Date)

---

<div align="center">

**如果这个项目对你有帮助，请给个 ⭐️ Star!**

Made with ❤️ by [zf13883922290](https://github.com/zf13883922290)

</div>
