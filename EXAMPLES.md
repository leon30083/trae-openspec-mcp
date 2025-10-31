# Trae-OpenSpec MCP 示例项目

本文档提供使用Trae-OpenSpec MCP工具的实际项目案例，展示如何从零开始创建完整的应用程序。

## 🎯 示例1：博客系统项目

### 项目需求
创建一个现代化的个人博客系统，支持Markdown编辑、标签分类、评论功能，要有美观的界面设计。

### 步骤1：创建项目

**使用创建项目工具：**
```json
{
  "description": "创建一个现代化的个人博客系统，支持Markdown编辑、标签分类、评论功能，要有美观的界面设计，支持响应式布局",
  "projectName": "ModernBlog",
  "techStack": "Next.js + TypeScript + PostgreSQL"
}
```

**生成的项目结构：**
```
ModernBlog/
├── openspec/
│   ├── specs/
│   │   ├── spec.md          # API规范文档
│   │   └── tasks.md         # 实现任务清单
├── src/
│   ├── components/          # React组件
│   │   ├── BlogPost.tsx
│   │   ├── Comment.tsx
│   │   └── TagFilter.tsx
│   ├── pages/              # Next.js页面
│   │   ├── index.tsx
│   │   ├── posts/[id].tsx
│   │   └── admin/
│   ├── api/                # API端点
│   │   ├── auth/
│   │   ├── posts/
│   │   └── comments/
│   └── lib/                # 工具函数
├── public/                 # 静态资源
├── package.json
└── README.md
```

### 步骤2：查看生成的规范

**spec.md 内容预览：**
```markdown
# API规范：现代博客平台

## 项目信息
- **名称**：现代博客平台
- **版本**：v1.0.0
- **描述**：支持用户注册、文章发布、评论功能的博客系统
- **技术栈**：Next.js + TypeScript + PostgreSQL

## 核心功能模块

### 1. 用户管理模块
#### API端点
- `POST /api/auth/register` - 用户注册
- `POST /api/auth/login` - 用户登录
- `GET /api/users/profile` - 获取用户资料
- `PUT /api/users/profile` - 更新用户资料

#### 数据模型
```typescript
interface User {
  id: string;
  username: string;
  email: string;
  avatar?: string;
  bio?: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### 2. 文章管理模块
#### API端点
- `GET /api/posts` - 获取文章列表（支持分页）
- `GET /api/posts/:id` - 获取文章详情
- `POST /api/posts` - 创建文章
- `PUT /api/posts/:id` - 更新文章
- `DELETE /api/posts/:id` - 删除文章

#### 数据模型
```typescript
interface Post {
  id: string;
  title: string;
  content: string;
  excerpt: string;
  tags: string[];
  authorId: string;
  published: boolean;
  createdAt: Date;
  updatedAt: Date;
}
```

### 3. 评论系统模块
#### API端点
- `GET /api/posts/:postId/comments` - 获取文章评论
- `POST /api/posts/:postId/comments` - 发表评论
- `PUT /api/comments/:id` - 更新评论
- `DELETE /api/comments/:id` - 删除评论
```

### 步骤3：实现代码

**使用AI生成代码：**
在Trae中打开项目后，可以使用以下提示词生成代码：

```
根据openspec/specs/spec.md文件中的规范，帮我实现用户注册功能，使用Next.js API Routes和Prisma ORM
```

**生成的代码示例：**
```typescript
// src/pages/api/auth/register.ts
import type { NextApiRequest, NextApiResponse } from 'next'
import bcrypt from 'bcryptjs'
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method !== 'POST') {
    return res.status(405).json({ message: 'Method not allowed' })
  }

  const { username, email, password } = req.body

  // 输入验证
  if (!username || !email || !password) {
    return res.status(400).json({ message: 'Missing required fields' })
  }

  // 验证邮箱格式
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(email)) {
    return res.status(400).json({ message: 'Invalid email format' })
  }

  try {
    // 检查用户名和邮箱是否已存在
    const existingUser = await prisma.user.findFirst({
      where: {
        OR: [
          { username },
          { email }
        ]
      }
    })

    if (existingUser) {
      return res.status(409).json({ message: 'Username or email already exists' })
    }

    // 密码加密
    const hashedPassword = await bcrypt.hash(password, 12)

    // 创建用户
    const user = await prisma.user.create({
      data: {
        username,
        email,
        password: hashedPassword,
      },
      select: {
        id: true,
        username: true,
        email: true,
        createdAt: true,
      }
    })

    res.status(201).json({
      message: 'User created successfully',
      user
    })
  } catch (error) {
    console.error('Registration error:', error)
    res.status(500).json({ message: 'Internal server error' })
  }
}
```

### 步骤4：优化代码

**使用优化代码工具：**
```json
{
  "code": "// 原始代码
async function getPosts(page = 1, limit = 10) {
  const posts = await prisma.post.findMany({
    skip: (page - 1) * limit,
    take: limit,
    include: {
      author: true,
      comments: true
    }
  })
  return posts
}",
  "spec": "# API规范\n## 文章管理\n- 分页处理\n- 性能优化\n- 缓存策略\n- 错误处理",
  "feedback": "需要添加分页元数据、性能优化、错误处理"
}
```

**优化后的代码：**
```typescript
/**
 * Get paginated posts with optimized queries
 * @param page - Current page number (default: 1)
 * @param limit - Posts per page (default: 10)
 * @returns Paginated posts with metadata
 */
async function getPosts(page = 1, limit = 10) {
  try {
    // Validate input parameters
    const validatedPage = Math.max(1, parseInt(page.toString()) || 1);
    const validatedLimit = Math.min(100, Math.max(1, parseInt(limit.toString()) || 10));
    
    const skip = (validatedPage - 1) * validatedLimit;

    // Execute queries in parallel for better performance
    const [posts, totalCount] = await Promise.all([
      // Get posts with optimized includes
      prisma.post.findMany({
        skip,
        take: validatedLimit,
        orderBy: { createdAt: 'desc' },
        include: {
          author: {
            select: {
              id: true,
              username: true,
              avatar: true,
            }
          },
          _count: {
            select: { comments: true }
          }
        }
      }),
      // Get total count for pagination
      prisma.post.count({
        where: { published: true }
      })
    ]);

    // Calculate pagination metadata
    const totalPages = Math.ceil(totalCount / validatedLimit);
    const hasNextPage = validatedPage < totalPages;
    const hasPrevPage = validatedPage > 1;

    return {
      data: posts,
      pagination: {
        page: validatedPage,
        limit: validatedLimit,
        totalCount,
        totalPages,
        hasNextPage,
        hasPrevPage,
        nextPage: hasNextPage ? validatedPage + 1 : null,
        prevPage: hasPrevPage ? validatedPage - 1 : null,
      }
    };
  } catch (error) {
    console.error('Error fetching posts:', error);
    throw new Error('Failed to fetch posts');
  }
}
```

## 🎯 示例2：电商平台项目

### 项目需求
开发一个完整的B2C电商平台，包含商品展示、购物车、订单处理、支付集成、库存管理功能。

### 步骤1：生成规范

**使用生成规范工具：**
```json
{
  "requirements": "B2C电商平台，包含商品管理、购物车、订单处理、支付集成、库存管理、用户管理、促销系统功能",
  "projectType": "ecommerce"
}
```

**生成的规范预览：**
```markdown
# API规范：电商平台

## 核心功能模块

### 1. 商品管理模块
#### API端点
- `GET /api/products` - 商品列表（支持筛选、排序、分页）
- `GET /api/products/:id` - 商品详情
- `POST /api/products` - 创建商品（管理员）
- `PUT /api/products/:id` - 更新商品（管理员）
- `DELETE /api/products/:id` - 删除商品（管理员）

### 2. 购物车模块
#### API端点
- `GET /api/cart` - 获取购物车
- `POST /api/cart/items` - 添加商品到购物车
- `PUT /api/cart/items/:id` - 更新购物车商品数量
- `DELETE /api/cart/items/:id` - 从购物车删除商品

### 3. 订单管理模块
#### API端点
- `POST /api/orders` - 创建订单
- `GET /api/orders/:id` - 订单详情
- `PUT /api/orders/:id/status` - 更新订单状态
- `GET /api/orders` - 订单历史
```

### 步骤2：创建项目

**使用创建项目工具：**
```json
{
  "description": "基于生成的规范创建完整的电商平台项目，包含前端Vue.js管理后台和Node.js API服务",
  "projectName": "EcommercePlatform",
  "techStack": "Vue.js + Node.js + MySQL + Redis"
}
```

## 🎯 示例3：企业管理系统

### 项目需求
企业级用户权限管理系统，支持组织架构、角色权限、单点登录、操作审计功能。

### 步骤1：生成规范

**使用生成规范工具：**
```json
{
  "requirements": "企业级用户权限管理系统，包含组织架构管理、角色权限控制、单点登录、操作日志审计、数据权限隔离功能",
  "projectType": "enterprise"
}
```

### 步骤2：创建项目

**使用创建项目工具：**
```json
{
  "description": "基于企业级规范创建用户权限管理系统，使用React前端和Spring Boot后端",
  "projectName": "EnterpriseAuthSystem",
  "techStack": "React + Spring Boot + PostgreSQL + Redis"
}
```

## 🎯 示例4：代码优化实战

### 场景：优化用户登录代码

**原始代码：**
```javascript
function login(username, password) {
  // 简单的登录验证
  const user = users.find(u => u.username === username && u.password === password);
  if (user) {
    return { success: true, user: user };
  } else {
    return { success: false, message: 'Login failed' };
  }
}
```

**使用优化代码工具：**
```json
{
  "code": "function login(username, password) {\n  const user = users.find(u => u.username === username && u.password === password);\n  if (user) {\n    return { success: true, user: user };\n  } else {\n    return { success: false, message: 'Login failed' };\n  }\n}",
  "spec": "# API规范\\n## 用户认证\\n- 输入验证\\n- 密码加密\\n- 错误处理\\n- 安全检查\\n- JWT Token生成",
  "feedback": "需要添加输入验证、密码加密、JWT Token生成、更详细的错误处理"
}
```

**优化后的代码：**
```javascript
/**
 * User login with enhanced security and validation
 * @param {string} username - User's username or email
 * @param {string} password - User's password (plain text)
 * @returns {Promise<Object>} Login result with token and user data
 */
async function login(username, password) {
  try {
    // Input validation
    if (!username || typeof username !== 'string' || username.trim().length === 0) {
      throw new Error('Username is required');
    }
    
    if (!password || typeof password !== 'string' || password.length < 6) {
      throw new Error('Password must be at least 6 characters long');
    }
    
    // Sanitize input
    const sanitizedUsername = username.trim().toLowerCase();
    
    // Find user (case-insensitive)
    const user = users.find(u => 
      u.username.toLowerCase() === sanitizedUsername || 
      u.email.toLowerCase() === sanitizedUsername
    );
    
    // Security: Don't reveal whether username exists
    if (!user) {
      throw new Error('Invalid credentials');
    }
    
    // Verify password (assuming bcrypt)
    const isPasswordValid = await bcrypt.compare(password, user.passwordHash);
    
    if (!isPasswordValid) {
      // Log failed attempt for security monitoring
      console.warn(`Failed login attempt for user: ${sanitizedUsername}`);
      throw new Error('Invalid credentials');
    }
    
    // Check account status
    if (user.isLocked || !user.isActive) {
      throw new Error('Account is locked or inactive');
    }
    
    // Generate JWT token
    const token = jwt.sign(
      { 
        userId: user.id, 
        username: user.username,
        role: user.role 
      },
      process.env.JWT_SECRET,
      { expiresIn: '24h' }
    );
    
    // Update last login time
    user.lastLoginAt = new Date();
    
    // Return sanitized user data
    const userData = {
      id: user.id,
      username: user.username,
      email: user.email,
      role: user.role,
      avatar: user.avatar,
      lastLoginAt: user.lastLoginAt
    };
    
    return {
      success: true,
      token,
      user: userData,
      expiresIn: '24h'
    };
    
  } catch (error) {
    console.error('Login error:', error.message);
    
    // Return generic error message for security
    return {
      success: false,
      message: error.message === 'Invalid credentials' 
        ? 'Invalid credentials' 
        : 'Login failed. Please try again.',
      code: error.message === 'Invalid credentials' ? 'INVALID_CREDENTIALS' : 'LOGIN_ERROR'
    };
  }
}
```

## 🚀 最佳实践总结

### 1. 项目创建流程

1. **明确需求描述**：提供详细的项目需求，包含功能、技术栈、设计偏好
2. **选择合适模板**：根据项目类型选择合适的模板
3. **检查生成结果**：仔细查看生成的项目结构和规范文件
4. **逐步完善**：根据需要调整和补充生成的内容

### 2. 规范生成技巧

1. **详细需求描述**：越详细的需求描述，生成的规范越准确
2. **选择项目类型**：使用预定义的项目类型可以获得更好的结果
3. **自定义调整**：生成的规范可以根据实际需要进行调整
4. **版本管理**：保存不同版本的规范，便于追踪变更

### 3. 代码优化策略

1. **提供完整代码**：确保提供的代码逻辑完整，便于分析
2. **明确优化目标**：给出具体的优化建议和方向
3. **逐步优化**：不要一次性优化太多内容，分步骤进行
4. **测试验证**：优化后的代码需要进行充分的测试

### 4. 集成开发流程

1. **创建项目**：使用MCP工具创建项目框架
2. **生成规范**：根据需求生成详细的API规范
3. **AI辅助开发**：在Trae中使用AI根据规范生成代码
4. **代码优化**：使用优化工具改进代码质量
5. **迭代完善**：根据测试结果持续优化

---

💡 **提示**：这些示例展示了MCP工具的强大功能，你可以根据实际需求灵活运用！

🎯 **下一步**：查看 [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) 了解常见问题和解决方案！