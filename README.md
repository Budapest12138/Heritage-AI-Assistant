# 墨韵 · 非遗知识助手 —— 介绍及一键部署指南

## 🚀 技术栈
- 前端:   vue + element UI 
- 后端：Spring Boot 3
- 数据库：MySQL + Redis
- AI模型：阿里云百炼（DashScope）
- 部署：Docker Compose

## ✨ 核心功能
1. 非遗知识智能问答（基于RAG检索增强）
2. 碑帖/非遗作品图片识别
3. 用户登录注册


> 使用阿里云容器镜像部署，无需源码，无需构建。

## 1. 前置条件

- 已安装 **Docker Desktop**（Windows/Mac）或 Docker Engine（Linux）
- 网络能访问阿里云镜像仓库（国内一般都可以）

## 2. 准备配置

复制环境变量模板：

```bash
cp .env.example .env
```

编辑 `.env`，填入**你自己的** DashScope API Key：

```env
DASHSCOPE_API_KEY=sk-你的Key
MYSQL_PASSWORD=Zz2005613   # 可改为任意密码
```

> API Key 获取：https://dashscope.console.aliyun.com/ （阿里云百炼控制台，创建 API-KEY）

## 3. 启动

```bash
docker compose up -d
```

首次启动会自动拉取镜像（镜像公开，无需登录阿里云），等待 `Started Demo4aiApplication` / `Started UlsApplication` 日志出现即完成。

## 4. 访问

| 入口 | 地址 |
|---|---|
| 前端页面 | http://localhost:8088 |
| AI 后端 | http://localhost:8080 |
| 登录接口 | http://localhost:8081 |
| Redis 管理界面 | http://localhost:8001 |

## 5. 常用命令

```bash
docker compose ps          # 查看状态
docker compose logs -f     # 查看日志
docker compose down        # 停止
docker compose up -d       # 重新启动
```

## 6. 常见问题

**Q: 前端页面能开，但 AI 不回复？**
检查 `.env` 中的 `DASHSCOPE_API_KEY` 是否正确，然后：
```bash
docker compose up -d --force-recreate heritage-backend
```

**Q: 端口冲突？**
修改 `docker-compose.yml` 中对应服务的 `ports` 映射（如 `8088:80` 改为 `8090:80`）。

**Q: 拉镜像慢/失败？**
阿里云个人实例镜像走公网，国内一般较快；如果慢可给 Docker 配置加速器（Docker Desktop → Settings → Docker Engine）：
```json
{
  "registry-mirrors": ["https://docker.m.daocloud.io"]
}
```

## 7. 镜像信息

| 镜像 | 说明 |
|---|---|
| `.../zayzeng/xiwen-frontend:latest` | 前端（Nginx） |
| `.../zayzeng/xiwen-backend:latest` | AI 问答/图像识别后端 |
| `.../zayzeng/xiwen-uls:latest` | 用户登录注册后端 |
| `redis/redis-stack:latest` | Redis（官方公共镜像） |
| `mysql:8.4` | MySQL（官方公共镜像） |
