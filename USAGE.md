# Trae-OpenSpec MCP 使用手册

## 🎯 概述

本手册详细介绍如何使用Trae-OpenSpec MCP工具的三个核心功能，帮助你快速创建和管理OpenSpec项目。

## 🛠️ 核心工具

### MCP 服务器配置（Trae JSON）

使用 Trae 的 JSON 配置方式添加 MCP 服务器（推荐使用已发布版 npx）：

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

说明：
- `cwd` 指向包含 `mcp-server.js` 的仓库根目录。
- 若未来发布到 npm，可切换为：

```json
{
  "mcpServers": {
    "trae-openspec-mcp": {
      "command": "node",
      "args": ["mcp-server.js"],
      "cwd": "<你的本地路径>/trae-openspec-mcp"
    }
  }
}
```

### 工具1：创建OpenSpec项目

#### 功能说明
基于自然语言描述自动创建完整的OpenSpec项目，包括规范文件、项目结构和实现任务。

#### 参数说明
| 参数名 | 类型 | 必需 | 描述 |
|--------|------|------|------|
| description | string | ✅ | 项目需求描述，越详细越好 |
| projectName | string | ✅ | 项目名称，建议使用英文或拼音 |
| techStack | string | ❌ | 技术栈偏好，如"React + Node.js" |

#### 使用示例

**示例1：创建博客系统**
```json
{
  "description": "创建一个现代化的个人博客系统，支持Markdown编辑、标签分类、评论功能，要有美观的界面设计",
  "projectName": "PersonalBlog",
  "techStack": "Next.js + TypeScript + PostgreSQL"
}
```

**示例2：创建电商平台**
```json
{
  "description": "开发一个完整的电商平台，包含商品展示、购物车、订单管理、支付功能，支持多用户角色",
  "projectName": "EcommercePlatform",
  "techStack": "Vue.js + Express + MongoDB"
}
```

**示例3：创建用户管理系统**
```json
{
  "description": "企业级用户管理系统，包含用户注册登录、权限管理、组织架构、操作日志功能",
  "projectName": "UserManagementSystem"
}
```

#### 返回结果
```
✅ OpenSpec项目 "PersonalBlog" 创建成功！

📝 项目描述：创建一个现代化的个人博客系统...
🏗️ 项目类型：blog
💻 技术栈：Next.js + TypeScript + PostgreSQL
🎯 复杂度：medium

📁 生成的文件结构：
  📁 PersonalBlog/
    📁 openspec/
      📁 specs/
        - spec.md: 包含完整的API规范
        - tasks.md: 包含详细的实现任务
    📁 src/
      📁 routes/
        - index.js: 路由配置
      📁 models/
        - index.js: 数据模型
    📄 package.json: 项目配置文件
    📄 README.md: 项目说明文档

📋 规范文件内容：
- spec.md: 包含完整的API规范
- tasks.md: 包含详细的实现任务

🎯 下一步：
1. 📖 查看生成的规范文件
2. 🤖 使用AI生成代码
3. 🧪 运行和测试项目
4. 📚 参考文档进行开发

💡 提示：在Trae中打开项目文件夹，开始开发！
```

### 工具2：生成OpenSpec规范

#### 功能说明
根据功能需求描述生成OpenSpec规范文件，可以选择项目类型或生成自定义规范。

#### 参数说明
| 参数名 | 类型 | 必需 | 描述 |
|--------|------|------|------|
| requirements | string | ✅ | 功能需求描述 |
| projectType | string | ❌ | 项目类型：blog/ecommerce/social/enterprise/custom |

#### 使用示例

**示例1：生成博客规范**
```json
{
  "requirements": "个人博客系统，支持文章发布、分类管理、评论功能，要有用户注册登录",
  "projectType": "blog"
}
```

**示例2：生成电商规范**
```json
{
  "requirements": "B2C电商平台，包含商品管理、购物车、订单处理、支付集成、库存管理功能",
  "projectType": "ecommerce"
}
```

**示例3：生成用户管理规范**
```json
{
  "requirements": "企业级用户权限管理系统，支持组织架构、角色权限、单点登录、操作审计",
  "projectType": "enterprise"
}
```

**示例4：生成自定义规范**
```json
{
  "requirements": "任务管理系统，包含任务创建、分配、进度跟踪、团队协作、通知提醒功能"
}
```

#### 返回结果
```markdown
# OpenSpec规范文件

基于您的需求生成的规范：

## 需求分析
个人博客系统，支持文章发布、分类管理、评论功能，要有用户注册登录

## 项目类型
blog

## 识别的特性
- user_management
- content_management
- comment_system

## 生成的规范内容
```markdown
# API规范：博客系统

## 项目信息
- **名称**：现代博客平台
- **版本**：v1.0.0
- **描述**：支持用户注册、文章发布、评论功能的博客系统
- **技术栈**：React + Node.js + PostgreSQL

## 核心功能模块

### 1. 用户管理模块
#### 功能描述
完整的用户注册、登录、个人资料管理功能

#### API端点
- `POST /api/auth/register` - 用户注册
- `POST /api/auth/login` - 用户登录
- `GET /api/users/profile` - 获取用户资料
- `PUT /api/users/profile` - 更新用户资料

#### 数据模型
```javascript
User {
  id: UUID,
  username: String (唯一),
  email: String (唯一),
  password: String (哈希),
  avatar: String (URL),
  bio: String,
  createdAt: DateTime,
  updatedAt: DateTime
}
```

### 2. 文章管理模块
#### 功能描述
文章的创建、编辑、发布、分类管理

#### API端点
- `GET /api/posts` - 获取文章列表
- `GET /api/posts/:id` - 获取文章详情
- `POST /api/posts` - 创建文章
- `PUT /api/posts/:id` - 更新文章
- `DELETE /api/posts/:id` - 删除文章

### 3. 评论系统模块
#### 功能描述
文章评论、回复、审核功能

#### API端点
- `GET /api/posts/:postId/comments` - 获取文章评论
- `POST /api/posts/:postId/comments` - 发表评论
- `PUT /api/comments/:id` - 更新评论
- `DELETE /api/comments/:id` - 删除评论

## 技术规范

### 安全要求
- JWT认证
- 输入验证和清理
- SQL注入防护
- XSS攻击防护
- 速率限制

### 性能要求
- 数据库查询优化
- 缓存策略
- 分页处理
- 图片压缩

### 错误处理
- 统一的错误响应格式
- 详细的错误日志
- 用户友好的错误消息
```

## 实现任务
```markdown
# 博客系统实现任务

## 🎯 第一阶段：项目初始化 (优先级：高)
### 1.1 环境搭建
- [ ] 创建项目目录结构
- [ ] 初始化Git仓库
- [ ] 配置开发环境 (Node.js, PostgreSQL)
- [ ] 安装核心依赖包

## 🎯 第二阶段：用户模块 (优先级：高)
### 2.1 用户认证
- [ ] 实现用户注册API
- [ ] 实现用户登录API
- [ ] 集成JWT Token生成和验证
- [ ] 添加密码加密和验证

## 🎯 第三阶段：文章模块 (优先级：高)
### 3.1 文章CRUD
- [ ] 创建文章模型和数据库表
- [ ] 实现文章创建API
- [ ] 实现文章列表获取API (支持分页)
- [ ] 实现文章详情获取API

## 🎯 第四阶段：评论模块 (优先级：中)
### 4.1 评论功能
- [ ] 创建评论数据模型
- [ ] 实现评论创建API
- [ ] 实现评论列表获取API
- [ ] 添加评论嵌套回复功能

## 🎯 第五阶段：优化和部署 (优先级：低)
### 5.1 性能优化
- [ ] 实现数据库查询优化
- [ ] 添加Redis缓存
- [ ] 实现图片懒加载
- [ ] 优化前端打包
```

📋 使用说明：
1. 📁 创建项目目录结构
2. 📝 复制上述规范内容到项目的spec.md文件
3. ✅ 复制任务内容到tasks.md文件
4. 🎯 根据需要调整细节
5. 🤖 使用AI生成代码实现

💡 提示：规范文件遵循OpenSpec标准，可直接用于代码生成。

🔧 高级用法：
- 结合项目模板使用
- 自定义规范内容
- 增量式规范更新
```

### 工具3：优化代码

#### 功能说明
基于OpenSpec规范优化现有代码，提供改进建议并生成优化后的代码。

#### 参数说明
| 参数名 | 类型 | 必需 | 描述 |
|--------|------|------|------|
| code | string | ✅ | 需要优化的代码 |
| spec | string | ✅ | OpenSpec规范内容 |
| feedback | string | ❌ | 优化反馈和建议 |

#### 使用示例

**示例1：优化用户认证代码**
```json
{
  "code": "function login(username, password) { return db.query('SELECT * FROM users WHERE username = ' + username + ' AND password = ' + password); }",
  "spec": "# API规范\n## 用户认证\n- 输入验证\n- 密码加密\n- SQL注入防护\n- 错误处理",
  "feedback": "需要添加输入验证和错误处理"
}
```

**示例2：优化API端点代码**
```json
{
  "code": "app.get('/api/users/:id', (req, res) => { const user = users.find(u => u.id == req.params.id); res.json(user); });",
  "spec": "# API规范\n## 用户管理\n- 参数验证\n- 错误处理\n- 数据过滤\n- 安全检查",
  "feedback": "添加404错误处理和数据验证"
}
```

**示例3：优化数据库查询代码**
```json
{
  "code": "async function getProducts() { const products = await db.query('SELECT * FROM products'); return products; }",
  "spec": "# API规范\n## 商品管理\n- 分页处理\n- 性能优化\n- 缓存策略\n- 错误处理"
}
```

#### 返回结果
```markdown
# 代码优化报告

## 📊 原始代码分析
- 📏 代码行数：5
- ✅ 规范符合度：65%
- ⚠️ 主要问题：需要添加错误处理, 缺少输入验证, 存在SQL注入风险
- 🎯 代码复杂度：low

## 💡 优化建议
1. 添加统一的错误处理中间件
2. 实现输入数据验证
3. 使用参数化查询防止SQL注入
4. 实现数据库访问层抽象
5. 优化数据库查询性能
6. 添加代码注释和文档

## 🔄 优化后的代码
```javascript
/**
 * Optimized function
 * Generated by Trae-OpenSpec MCP
 * Timestamp: 2024-01-15T10:30:00.000Z
 */

// Input validation
if (!username || typeof username !== 'string' || !password || typeof password !== 'string') {
  throw new Error('Invalid input: username and password are required');
}

// Additional validation
if (username.length < 3 || username.length > 50) {
  throw new Error('Username must be between 3 and 50 characters');
}

try {
  // Use parameterized query to prevent SQL injection
  const query = 'SELECT * FROM users WHERE username = $1 AND password = $2';
  const result = await db.query(query, [username, password]);
  
  // Return the first user found or null
  return result.rows[0] || null;
} catch (error) {
  console.error('Database query failed:', error.message);
  throw new Error('Login failed: Unable to authenticate user');
}
```

## 📈 改进总结
✅ **已完成的优化：**
- 改进了代码结构和可读性
- 确保符合OpenSpec规范
- 添加了必要的错误处理
- 优化了性能考虑
- 增强了安全性
- 添加了详细的代码注释

💡 **进一步建议：**
1. 🧪 测试优化后的代码
2. 📊 根据实际运行结果进一步调整
3. 🔄 保持代码与规范的同步更新
4. 📚 参考最佳实践持续改进

🔧 **技术细节：**
- 优化时间：2024/1/15 10:30:00
- 处理耗时：1.2s
- 优化级别：智能分析 + 规范匹配
```

## 📋 使用流程

### 场景1：从零开始创建项目

1. **描述项目需求**
   ```
   我想创建一个任务管理系统，包含任务创建、分配、进度跟踪功能
   ```

2. **使用创建项目工具**
   ```json
   {
     "description": "任务管理系统，包含任务创建、分配、进度跟踪、团队协作功能",
     "projectName": "TaskManager",
     "techStack": "React + Express + PostgreSQL"
   }
   ```

3. **查看生成的项目结构**
4. **在Trae中打开项目文件夹**
5. **根据规范文件开始开发**

### 场景2：为现有项目生成规范

1. **分析项目需求**
2. **使用生成规范工具**
   ```json
   {
     "requirements": "现有CRM系统需要添加客户管理、销售跟踪、报表分析功能",
     "projectType": "enterprise"
   }
   ```
3. **复制生成的规范内容**
4. **应用到现有项目中**

### 场景3：优化现有代码

1. **准备需要优化的代码**
2. **准备对应的规范内容**
3. **使用优化代码工具**
   ```json
   {
     "code": "你的原始代码",
     "spec": "对应的规范内容",
     "feedback": "具体的优化建议"
   }
   ```
4. **应用优化后的代码**
5. **测试和验证优化结果**

## 💡 最佳实践

### 1. 项目创建最佳实践

- **详细描述需求**：提供尽可能详细的功能描述
- **选择合适的技术栈**：根据项目需求选择技术栈
- **使用有意义的项目名称**：使用简洁明了的项目名称
- **检查生成的规范**：仔细查看生成的规范文件是否符合预期

### 2. 规范生成最佳实践

- **明确项目类型**：选择合适的项目类型以获得更准确的规范
- **提供完整的需求描述**：包含所有关键功能需求
- **自定义规范内容**：根据实际需要调整生成的规范
- **保持规范更新**：随着项目进展更新规范文件

### 3. 代码优化最佳实践

- **提供完整的代码**：确保代码逻辑完整，便于分析
- **提供相关的规范**：提供与代码相关的OpenSpec规范
- **给出具体的反馈**：提供明确的优化建议
- **逐步优化**：不要一次性优化太多内容

## 🎯 高级技巧

### 1. 批量创建项目

通过组合多个工具调用，可以快速创建多个相关项目：

1. 创建主项目
2. 为每个子模块生成规范
3. 分别创建子项目

### 2. 规范模板化

将常用的规范内容保存为模板，便于重复使用：

1. 生成完整的规范
2. 提取通用部分作为模板
3. 在新项目中复用模板

### 3. 代码优化策略

- **渐进式优化**：先解决主要问题，再处理细节
- **规范驱动优化**：严格按照规范要求进行优化
- **性能优先**：优先优化性能瓶颈
- **安全加固**：重点优化安全相关的代码

## 🔧 故障排除

### 常见问题1：项目创建失败

**症状**：工具返回错误信息

**解决方案**：
- 检查项目名称是否包含特殊字符
- 确认需求描述不为空
- 验证技术栈格式是否正确

### 常见问题2：规范生成不准确

**症状**：生成的规范与需求不符

**解决方案**：
- 提供更详细的需求描述
- 选择合适的项目类型
- 手动调整生成的规范内容

### 常见问题3：代码优化效果不佳

**症状**：优化后的代码没有明显改善

**解决方案**：
- 提供更具体的优化反馈
- 确保规范内容与代码相关
- 分多次进行优化

---

💡 **提示**：MCP工具完全集成在Trae中，所有操作都可以通过自然语言对话完成！

🚀 **下一步**：查看 [EXAMPLES.md](./EXAMPLES.md) 了解实际项目案例！