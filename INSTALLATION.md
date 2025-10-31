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

### 1. 基础配置（可选）

创建 `mcp-config.json`（当前版本未由服务器读取，仅用于规划与记录）：

```json
{
  "server": {
    "name": "trae-openspec-server",
    "version": "0.1.0",
    "timeout": 30000
  }
}
```

### 2. 环境变量

创建 `.env` 文件（如需真实写盘创建文件）：

```bash
# 文件创建选项（false 表示仅生成虚拟文件结构）
CREATE_REAL_FILES=false
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

**问题**：启动后无输出或进程立即退出

**解决方案（Windows 10/11）：**
```bash
# 以本地仓库方式启动
npm start

# 以开发模式（自动重启）启动
npm run dev

# 检查Node版本与路径
node -v
where node
```

### 常见问题3：Trae无法连接MCP服务器

**问题**：Trae显示连接失败

**解决方案（STDIO 模式）：**
1. 在Trae设置中点击“测试连接”，确认状态为“已连接”。
2. 检查JSON配置的 `command`/`args`/`cwd` 是否指向正确的脚本与目录。
3. 终止当前进程（Ctrl+C）后重新运行 `npm start`。
4. 检查安全软件是否拦截 Trae 或 Node 进程。

### 常见问题4：工具调用失败

**问题**：工具返回错误信息

**解决方案：**
- 重新核对工具入参格式是否符合 README 中的使用说明。
- 使用开发模式启动以观察实时输出：`npm run dev`。
- 尝试最小化输入（仅必要参数），排除复杂上下文影响。

## 📊 性能监控

当前版本未内置状态/基准测试命令。建议：
- 使用开发模式（`npm run dev`）观察实时输出与错误栈。
- 在Trae中逐一执行工具，评估响应时间与稳定性。

## 🔐 安全建议

### 1. 生产环境配置

- 验证与清洗所有外部输入（尤其是模板变量）。
- 控制生成文件的目录与权限，避免覆盖敏感路径。
- 审计关键操作（如真实写盘时记录文件列表与时间戳）。

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

1. **查看控制台输出**：启动窗口的实时日志
2. **开发模式**：`npm run dev`（自动重启，便于观察）
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