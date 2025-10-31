# Trae-OpenSpec MCP工具套件

🚀 **零命令行**的OpenSpec项目创建和管理工具，专为Trae AI设计。

## 🎯 功能特性

- **🎨 图形化操作**：完全基于Trae的GUI界面，无需命令行
- **⚡ 一键创建**：通过自然语言描述自动生成完整项目
- **📋 智能规范**：AI驱动的规范文件生成
- **🔧 代码优化**：基于OpenSpec规范的智能代码优化
- **🧪 实时测试**：内置测试和验证功能

## 📦 快速开始

### 1. 环境要求

- **Node.js**: v18.0.0 或更高版本
- **Trae AI**: 最新版本
- **操作系统**: Windows/macOS/Linux

### 2. 安装方式

#### 方法1：快速安装（推荐）

```bash
# 1. 克隆MCP工具仓库
git clone https://github.com/leon30083/trae-openspec-mcp.git
cd trae-openspec-mcp

# 2. 安装依赖
npm install

# 3. 启动服务器
npm start
```

#### 方法2：全局安装

```bash
# 1. 全局安装MCP工具
npm install -g trae-openspec-mcp

# 2. 在任何目录启动服务器
trae-openspec-mcp
```

#### 方法3：开发模式安装

```bash
# 1. 克隆仓库
git clone https://github.com/leon30083/trae-openspec-mcp.git
cd trae-openspec-mcp

# 2. 安装开发依赖
npm install

# 3. 以开发模式启动（带调试信息）
npm run dev
```

### 3. 启动MCP服务器

可按安装方式选择对应的启动命令：

```bash
# A) 本地仓库启动（推荐）
npm start
# 或直接使用 Node
node mcp-server.js

# B) 全局安装后启动（CLI）
trae-openspec-mcp

# C) 开发模式（自动重启）
npm run dev
```

提示：本MCP服务器采用STDIO传输与Trae集成，无需HTTP端口配置。

### 4. 在Trae中配置

1. 打开Trae AI设置
2. 进入"MCP服务器"配置
3. 添加新的MCP服务器
使用 JSON 方式添加 MCP 服务器，在 Trae 的配置中加入：

```json
{
  "mcpServers": {
    "trae-openspec-mcp": {
      "command": "node",
      "args": [
        "mcp-server.js"
      ],
      "cwd": "<你的本地路径>/trae-openspec-mcp"
    }
  }
}
```

说明：
- 将 `cwd` 设置为仓库根目录（包含 `mcp-server.js`）。
- 若已发布到 npm，可改为：

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

**路径说明（Windows）**
- 本项目在你的环境中的本地路径为：`F:\\Cursor\\OpenSpec\\mcp-tools`
- 在 JSON 中使用 Windows 路径时，请使用双反斜杠 `\\`
- 使用 `npx` 配置时可以省略 `cwd`，由 npm 解析安装目录
## 🛠️ 核心工具

### 1. 创建OpenSpec项目

**功能**：基于自然语言描述自动创建完整的OpenSpec项目

**参数**：
- `description` (必需): 项目需求描述
- `projectName` (必需): 项目名称
- `techStack` (可选): 技术栈偏好

**示例**：
```json
{
  "description": "创建一个现代化的博客系统，支持用户注册、文章发布、评论功能",
  "projectName": "MyBlog",
  "techStack": "React + Node.js"
}
```

**返回**：
- 项目结构
- 生成的规范文件
- 下一步操作指南

### 2. 生成OpenSpec规范

**功能**：根据需求描述生成OpenSpec规范文件

**参数**：
- `requirements` (必需): 功能需求描述
- `projectType` (可选): 项目类型 (blog/ecommerce/social/enterprise/custom)

**示例**：
```json
{
  "requirements": "用户管理系统，包含注册、登录、个人资料管理功能",
  "projectType": "user_management"
}
```

**返回**：
- 完整的spec.md内容
- 对应的tasks.md内容
- 使用说明

### 3. 优化代码

**功能**：基于OpenSpec规范优化现有代码

**参数**：
- `code` (必需): 需要优化的代码
- `spec` (必需): OpenSpec规范内容
- `feedback` (可选): 优化反馈和建议

**示例**：
```json
{
  "code": "function getUser(id) { return db.query('SELECT * FROM users WHERE id = ' + id); }",
  "spec": "# API规范\n## 用户管理\n- 输入验证\n- 错误处理\n- SQL注入防护",
  "feedback": "需要添加输入验证和错误处理"
}
```

**返回**：
- 优化后的代码
- 优化报告
- 改进建议

## 📋 使用流程

### 场景1：创建新项目

1. **描述需求**：在Trae中描述你的项目需求
2. **生成项目**：使用`create_openspec_project`工具
3. **查看结果**：检查生成的项目结构和规范
4. **开始开发**：在Trae中打开项目进行开发

### 场景2：生成规范

1. **提供需求**：描述你的功能需求
2. **选择类型**：指定项目类型或保持默认
3. **生成规范**：使用`generate_openspec_spec`工具
4. **应用规范**：将生成的规范应用到项目中

### 场景3：优化代码

1. **提供代码**：粘贴需要优化的代码
2. **提供规范**：粘贴对应的OpenSpec规范
3. **添加反馈**：（可选）提供优化建议
4. **获取优化**：使用`optimize_code_by_spec`工具

## 🎯 项目模板

MCP工具内置了多种项目模板：

### 博客系统 (blog)
- 用户注册/登录
- 文章CRUD操作
- 评论系统
- 富文本编辑器

### 电商平台 (ecommerce)
- 商品管理
- 购物车系统
- 订单处理
- 支付集成

### 用户管理 (user_management)
- 认证授权
- 用户资料管理
- 权限控制
- JWT Token管理

### 自定义项目 (custom)
- 基于需求智能生成
- 灵活的功能组合
- 可定制的规范模板

## 🔧 配置选项

### 服务器配置

创建 `mcp-config.json` 文件来自定义服务器行为：

```json
{
  "server": {
    "name": "trae-openspec-server",
    "version": "0.1.0",
    "timeout": 30000
  },
  "templates": {
    "customTemplatesPath": "./custom-templates",
    "enableCustomTemplates": true
  },
  "ai": {
    "model": "gpt-4",
    "temperature": 0.7,
    "maxTokens": 2000
  }
}
```

### 自定义模板

在 `custom-templates` 目录中添加你的模板：

```
custom-templates/
├── blog/
│   ├── spec.md
│   └── tasks.md
├── ecommerce/
│   ├── spec.md
│   └── tasks.md
└── [your-template]/
    ├── spec.md
    └── tasks.md
```

## 🐛 故障排除

### 常见问题

**Q: MCP服务器无法启动**
- 检查Node.js版本是否符合要求
- 确认所有依赖已正确安装
- 查看控制台错误信息

**Q: Trae无法连接MCP服务器**
- 检查服务器配置是否正确
- 确认工作目录设置正确
- 验证网络连接状态

**Q: 工具调用失败**
- 检查参数格式是否正确
- 确认必需参数已提供
- 查看控制台输出获取详细信息

### 调试模式

开发模式会自动重启并输出详细日志：

```bash
npm run dev
```

## 📚 示例项目

### 示例1：创建博客系统

```json
// 请求
{
  "name": "create_openspec_project",
  "arguments": {
    "description": "创建一个现代化的个人博客系统，支持Markdown编辑、标签分类、评论功能",
    "projectName": "PersonalBlog",
    "techStack": "Next.js + TypeScript"
  }
}

// 响应
{
  "content": [{
    "type": "text",
    "text": "✅ OpenSpec项目 \"PersonalBlog\" 创建成功！\n\n📁 生成的文件结构：\n  📁 PersonalBlog/\n    📁 openspec/\n      📁 specs/\n        - spec.md: 博客系统API规范\n        - tasks.md: 实现任务清单\n    📁 src/\n      📁 routes/\n        - index.js: 路由配置\n      📁 models/\n        - index.js: 数据模型\n    📄 package.json: 项目配置\n    📄 README.md: 项目说明\n\n🎯 下一步：\n1. 查看生成的规范文件\n2. 使用AI生成代码\n3. 运行和测试项目"
  }]
}
```

### 示例2：生成用户管理规范

```json
// 请求
{
  "name": "generate_openspec_spec",
  "arguments": {
    "requirements": "企业级用户管理系统，包含组织架构、角色权限、单点登录功能",
    "projectType": "enterprise"
  }
}

// 响应
{
  "content": [{
    "type": "text",
    "text": "# OpenSpec规范文件\n\n基于您的需求生成的规范：\n\n## 需求分析\n企业级用户管理系统，包含组织架构、角色权限、单点登录功能\n\n## 生成的规范内容\n```markdown\n# API规范：企业用户管理系统\n\n## 项目信息\n- 名称：企业用户管理系统\n- 版本：v1.0.0\n- 描述：支持组织架构、角色权限、单点登录的企业级系统\n\n## 核心功能\n### 1. 组织架构管理\n- 部门管理\n- 员工信息管理\n- 组织层级结构\n\n### 2. 角色权限系统\n- 角色定义和管理\n- 权限分配\n- 资源访问控制\n\n### 3. 单点登录(SSO)\n- OAuth2集成\n- SAML支持\n- 统一身份认证\n```\n\n📋 使用说明：\n1. 复制上述规范内容到项目的spec.md文件\n2. 根据需要调整细节\n3. 使用AI生成代码实现"
  }]
}
```

## 🤝 贡献指南

欢迎贡献！请查看 [CONTRIBUTING.md](./CONTRIBUTING.md) 了解详情。

## 📄 许可证

MIT License - 查看 [LICENSE](./LICENSE) 文件了解详情。

## 🔗 相关链接

- [OpenSpec官方文档](https://openspec.dev)
- [Trae AI官网](https://trae.ai)
- [MCP协议规范](https://modelcontextprotocol.io)

---

💡 **提示**：本工具完全兼容Trae AI的图形化界面，无需记忆任何命令行指令！