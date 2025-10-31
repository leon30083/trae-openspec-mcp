# Trae-OpenSpec MCP 安装指南

## 🎯 概述

本指南将帮助你快速安装和配置Trae-OpenSpec MCP服务器，实现零命令行的OpenSpec项目创建和管理。

## 📋 系统要求

### 必需环境
- **Node.js**: v18.0.0 或更高版本
- **npm**: v7.0.0 或更高版本
- **操作系统**: Windows 10/11, macOS 10.15+, Ubuntu 20.04+

### 推荐环境
- **Node.js**: v20.0.0 或更高版本 (LTS版本)
- **npm**: v9.0.0 或更高版本
- **内存**: 至少 4GB RAM
- **存储**: 至少 1GB 可用磁盘空间

## 🔧 安装步骤

### 方法1：快速安装（推荐）

```bash
# 1. 克隆MCP工具仓库
git clone https://github.com/leon30083/trae-openspec-mcp.git
cd trae-openspec-mcp

# 2. 安装依赖
npm install

# 3. 启动服务器
npm start
```

### 方法2：全局安装

```bash
# 1. 全局安装MCP工具
npm install -g trae-openspec-mcp

# 2. 在任何目录启动服务器
trae-openspec-mcp
```

### 方法3：开发模式安装

```bash
# 1. 克隆仓库
git clone https://github.com/leon30083/trae-openspec-mcp.git
cd trae-openspec-mcp

# 2. 安装开发依赖
npm install

# 3. 以开发模式启动（带调试信息）
npm run dev
```

## ⚙️ 环境配置

### 1. 基础配置

创建配置文件 `mcp-config.json`：

```json
{
  "server": {
    "name": "trae-openspec-server",
    "version": "0.1.0",
    "timeout": 30000,
    "maxRequests": 1000
  },
  "logging": {
    "level": "info",
    "file": "logs/mcp-server.log"
  },
  "templates": {
    "customTemplatesPath": "./custom-templates",
    "enableCustomTemplates": true
  }
}
```

### 2. 环境变量

创建 `.env` 文件：

```bash
# 服务器配置
MCP_SERVER_PORT=3000
MCP_SERVER_HOST=localhost

# 日志配置
LOG_LEVEL=info
LOG_FILE=logs/mcp-server.log

# 文件创建选项
CREATE_REAL_FILES=false

# 调试模式
DEBUG=mcp:*
```

### 3. 自定义模板配置

创建自定义模板目录结构：

```
custom-templates/
├── blog/
│   ├── spec.md
│   └── tasks.md
├── ecommerce/
│   ├── spec.md
│   └── tasks.md
└── your-template/
    ├── spec.md
    └── tasks.md
```

## 🔌 Trae集成配置

### 1. 打开Trae设置

1. 启动Trae AI
2. 点击右上角设置图标
3. 选择"MCP服务器"选项

### 2. 添加MCP服务器（JSON方式）

在 Trae 的配置中加入如下 JSON（推荐使用已发布版 npx）：

```json
{
  "mcpServers": {
    "trae-openspec-mcp": {
      "command": "npx",
      "args": ["-y", "trae-openspec-mcp"]
    }
  }
}
```

说明：
- 使用 `npx` 时可省略 `cwd`，由 npm 解析安装目录。
- Windows 路径注意事项：若使用本地开发模式，请在 JSON 中使用双反斜杠 `\\`。

如需本地开发模式（克隆仓库后运行），使用 `node + cwd`：

```json
{
  "mcpServers": {
    "trae-openspec-mcp": {
      "command": "node",
      "args": ["mcp-server.js"],
      "cwd": "F\\\\Cursor\\\\OpenSpec\\\\mcp-tools"
    }
  }
}
```

### 3. 测试连接

点击"测试连接"按钮，确认服务器状态为"已连接"。

### 4. 验证工具

在Trae对话中输入：
```
列出可用的MCP工具
```

应该能看到三个工具：
- `create_openspec_project`
- `generate_openspec_spec`
- `optimize_code_by_spec`

## 🚀 验证安装

### 测试工具1：创建项目

在Trae中输入：
```
使用MCP工具创建一个博客项目，名称为"TestBlog"
```

### 测试工具2：生成规范

在Trae中输入：
```
使用MCP工具生成一个用户管理系统的规范
```

### 测试工具3：优化代码

在Trae中输入：
```
使用MCP工具优化以下代码：[粘贴你的代码]
```

## 🔧 高级配置

### 1. 性能优化

编辑 `mcp-config.json`：

```json
{
  "performance": {
    "cacheEnabled": true,
    "cacheTTL": 3600,
    "maxConcurrentRequests": 50,
    "requestTimeout": 10000
  },
  "ai": {
    "model": "gpt-4",
    "temperature": 0.7,
    "maxTokens": 2000,
    "timeout": 30000
  }
}
```

### 2. 安全设置

```json
{
  "security": {
    "enableRateLimit": true,
    "maxRequestsPerMinute": 60,
    "enableCORS": true,
    "allowedOrigins": ["https://trae.ai"]
  }
}
```

### 3. 自定义模板

在 `custom-templates` 目录中创建模板：

```markdown
# custom-templates/my-template/spec.md
# API规范：我的自定义项目

## 项目信息
- 名称：{{projectName}}
- 版本：v1.0.0
- 描述：{{description}}

## 自定义内容
[在这里添加你的规范内容]
```

```markdown
# custom-templates/my-template/tasks.md
# 实现任务

## 第一阶段：项目初始化
- [ ] 创建项目结构
- [ ] 配置开发环境
- [ ] 根据需求实现功能

## 自定义任务
[在这里添加你的任务]
```

## 🐛 故障排除

### 常见问题1：依赖安装失败

**问题**：`npm install` 报错

**解决方案**：
```bash
# 清除npm缓存
npm cache clean --force

# 删除node_modules并重新安装
rm -rf node_modules package-lock.json
npm install

# 或者使用yarn
yarn install
```

### 常见问题2：MCP服务器无法启动

**问题**：启动时报端口占用或权限错误

**解决方案**：
```bash
# 检查端口占用
netstat -ano | findstr :3000

# 使用不同端口
MCP_SERVER_PORT=3001 npm start

# 检查文件权限
chmod +x mcp-server.js
```

### 常见问题3：Trae无法连接MCP服务器

**问题**：Trae显示连接失败

**解决方案**：
1. 确认服务器已启动
2. 检查工作目录路径是否正确
3. 验证防火墙设置
4. 查看服务器日志获取详细信息

### 常见问题4：工具调用失败

**问题**：工具返回错误信息

**解决方案**：
```bash
# 启用调试模式
DEBUG=mcp:* npm start

# 查看详细日志
tail -f logs/mcp-server.log
```

## 📊 性能监控

### 查看服务器状态

```bash
# 检查服务器进程
ps aux | grep mcp-server

# 查看内存使用
npm run status

# 性能测试
npm run benchmark
```

### 日志分析

```bash
# 查看实时日志
tail -f logs/mcp-server.log

# 搜索错误日志
grep ERROR logs/mcp-server.log

# 统计工具使用频率
grep "Tool called" logs/mcp-server.log | wc -l
```

## 🔐 安全建议

### 1. 生产环境配置

```json
{
  "security": {
    "enableHTTPS": true,
    "validateInputs": true,
    "sanitizeOutputs": true,
    "enableAuditLog": true
  }
}
```

### 2. 访问控制

- 限制MCP服务器只能被Trae访问
- 使用防火墙规则限制IP访问
- 定期更新依赖包修复安全漏洞

### 3. 数据保护

- 不要在日志中记录敏感信息
- 使用环境变量存储配置信息
- 定期备份重要数据

## 🔄 更新和维护

### 检查更新

```bash
# 检查新版本
npm outdated

# 更新到最新版本
npm update

# 全局更新
npm update -g trae-openspec-mcp
```

### 备份配置

```bash
# 备份配置文件
cp mcp-config.json mcp-config.json.backup
cp .env .env.backup

# 备份自定义模板
cp -r custom-templates custom-templates-backup
```

## 📞 技术支持

### 获取帮助

1. **查看日志**：`logs/mcp-server.log`
2. **启用调试**：`DEBUG=mcp:* npm start`
3. **社区支持**：访问项目GitHub Issues
4. **文档更新**：定期查看官方文档

### 报告问题

报告问题时请提供：
- 操作系统和版本
- Node.js版本
- 错误日志
- 复现步骤
- 配置文件（去除敏感信息）

---

💡 **提示**：安装完成后，建议先运行测试用例验证安装是否成功！

🚀 **下一步**：查看 [USAGE.md](./USAGE.md) 了解如何使用MCP工具