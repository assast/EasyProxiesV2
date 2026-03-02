# EasyProxiesV2

这是一个轻量级、高性能的代理池与订阅管理工具，底层基于强大的 [sing-box](https://github.com/SagerNet/sing-box) 核心构建。自带现代化的 Web 管理控制台，支持自动化的节点健康检查、多协议代理分发、订阅管理以及可视化的流量与延迟监控。

> **二开声明**：本项目基于大佬的 [jasonwong1991/easy_proxies](https://github.com/jasonwong1991/easy_proxies) 项目进行二次开发。V2 版本主要将前端重构为现代化的 React 单页应用，优化了全栈的交互体验，并改进了项目的工程化构建流程。

## ✨ 核心特性

- 🚀 **现代化的 Web UI**：采用 React 19 + Vite + TailwindCSS + DaisyUI 构建的响应式控制台，支持丰富的数据图表展示（节点延迟排行、流量排行、区域分布等）。
- 📦 **极简部署 (All-in-One)**：前端静态资源通过 `go:embed` 直接打包入 Go 二进制文件中，只需运行一个文件即可同时提供代理服务与 Web 面板，无需额外部署 Web 服务器。
- 🔄 **智能代理池与订阅**：支持多种协议节点的导入、订阅链接的定时拉取与解析；自动进行连通性测试、延迟检测以及故障节点自动隔离。
- 🛡️ **灵活的路由与分发**：支持单端口智能分发、多端口映射，内置 GeoIP 分流规则支持。
- 📊 **持久化数据监控**：使用内置 SQLite 持久化存储节点统计数据、连通性日志和流量消耗。
- 🐳 **容器化与安全**：提供多阶段优化的 Docker 镜像，默认以非 root (`easy`) 用户安全运行。

## 🛠️ 构建与开发

本项目由 Go (1.24+) 后端和 Node (22+) 前端组成。

### 1. 本地手动编译

**编译前端：**
```bash
cd frontend
npm ci
npm run build
# 构建产物会自动输出到 ../internal/monitor/assets 目录
```

**编译后端 (包含打包好的前端)：**
```bash
# 回到项目根目录
go mod download
go build -tags "with_utls with_quic with_grpc with_wireguard with_gvisor" -o easy-proxies ./cmd/easy_proxies
```

### 2. Docker 构建

项目已提供开箱即用的 `Dockerfile` 和 `docker-compose.yml`，推荐使用 Docker 进行部署：

```bash
docker build -t easy-proxies:latest .
```

## 🚀 部署指南

使用 Docker Compose 部署是最简单的方式：

1. 准备配置文件：
   将项目中的 `config.example.yaml` 复制为 `config.yaml`，并根据实际需求修改（如设置管理面板密码、修改端口等）。

2. 启动服务：
   ```bash
   docker compose up -d
   ```

3. 访问 Web 面板：
   打开浏览器访问 `http://<你的IP>:9091` (默认管理端口，可在 config.yaml 中修改)。

## 📁 目录结构简述

- `cmd/easy_proxies/`：Go 程序主入口。
- `frontend/`：React 源码，包含仪表盘、设置、节点管理等 UI 组件。
- `internal/`：后端的各个核心模块（如代理池、监控、配置、GeoIP、订阅管理等）。
- `internal/monitor/assets/`：存放前端 Vite 构建后的静态资源文件，供 Go 嵌入。
- `Dockerfile` / `docker-compose.yml`：容器化部署文件。

## 🙏 鸣谢

- 原作者 [jasonwong1991/easy_proxies](https://github.com/jasonwong1991/easy_proxies)
- 核心代理引擎 [sing-box](https://github.com/SagerNet/sing-box)
