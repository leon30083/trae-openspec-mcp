# Trae-OpenSpec MCP 故障排除指南

本文档提供Trae-OpenSpec MCP工具使用过程中常见问题的诊断和解决方案。

## 🔧 安装问题

### 问题1：MCP服务器启动失败

**错误信息：**
```
Error: Cannot find module '@modelcontextprotocol/sdk'
```

**解决方案：**
1. 确保已安装所有依赖：
   ```bash
   npm install
   ```

2. 检查Node.js版本（需要v18.0.0或更高）：
   ```bash
   node --version
   ```

3. 重新安装依赖：
   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

### 问题2：权限错误

**错误信息：**
```
Error: EACCES: permission denied, open 'server.log'
```

**解决方案：**
1. 检查文件权限：
   ```bash
   ls -la
   ```

2. 修改文件权限：
   ```bash
   chmod 755 .
   ```

3. 以管理员身份运行（不推荐长期使用）：
   ```bash
   sudo npm start
   ```

### 问题3：端口被占用

**错误信息：**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**解决方案：**
1. 查找占用端口的进程：
   ```bash
   # Windows
   netstat -ano | findstr :3000
   
   # macOS/Linux
   lsof -i :3000
   ```

2. 终止占用进程：
   ```bash
   # Windows
   taskkill /PID <PID> /F
   
   # macOS/Linux
   kill -9 <PID>
   ```

3. 修改配置文件使用其他端口

## 🔧 连接问题

### 问题1：Trae无法连接MCP服务器

**症状：**
- Trae中MCP工具无响应
- 工具调用超时

**诊断步骤：**
1. 检查MCP服务器状态：
   ```bash
   npm run status
   ```

2. 查看服务器日志：
   ```bash
   tail -f logs/server.log
   ```

3. 测试网络连接：
   ```bash
   curl http://localhost:3000/health
   ```

**解决方案：**
1. 重启MCP服务器：
   ```bash
   npm restart
   ```

2. 检查防火墙设置
3. 确认Trae配置正确

### 问题2：工具调用失败

**错误信息：**
```
Tool execution failed: Invalid parameters
```

**解决方案：**
1. 检查参数格式是否符合JSON格式
2. 确认必需参数都已提供
3. 验证参数类型是否正确
4. 查看参数长度限制

### 问题3：超时错误

**错误信息：**
```
Tool execution timeout after 30 seconds
```

**解决方案：**
1. 简化项目需求描述
2. 减少生成的文件数量
3. 分批处理大型项目
4. 增加超时时间配置

## 🔧 工具使用问题

### 工具1：创建OpenSpec项目

#### 问题1：项目创建失败

**症状：**
- 项目目录未创建
- 文件生成不完整

**可能原因：**
1. 项目路径包含特殊字符
2. 磁盘空间不足
3. 权限不足

**解决方案：**
1. 使用简单的项目名称：
   ```json
   {
     "projectName": "MyProject",
     "description": "项目描述",
     "techStack": "React + Node.js"
   }
   ```

2. 检查磁盘空间：
   ```bash
   df -h
   ```

3. 确保有写入权限

#### 问题2：生成的规范不符合预期

**症状：**
- 缺少关键功能
- 技术栈不匹配

**解决方案：**
1. 提供更详细的需求描述：
   ```json
   {
     "description": "创建一个电商网站，包含以下功能：1. 商品展示和搜索 2. 用户注册登录 3. 购物车功能 4. 订单管理 5. 支付集成。使用React前端和Node.js后端，MySQL数据库",
     "projectName": "EcommerceSite"
   }
   ```

2. 明确指定技术栈：
   ```json
   {
     "techStack": "Vue.js + Express + PostgreSQL",
     "description": "项目描述"
   }
   ```

### 工具2：生成OpenSpec规范

#### 问题1：规范生成不完整

**症状：**
- 缺少API端点
- 数据模型不完整

**解决方案：**
1. 提供更详细的需求描述：
   ```json
   {
     "requirements": "用户管理系统，包含：1. 用户注册（需要邮箱验证）2. 用户登录（支持JWT）3. 用户资料管理（头像上传）4. 密码重置功能 5. 用户权限管理（管理员、普通用户角色）",
     "projectType": "enterprise"
   }
   ```

2. 选择合适的项目类型：
   - `blog` - 博客系统
   - `ecommerce` - 电商平台
   - `social` - 社交网络
   - `enterprise` - 企业系统
   - `custom` - 自定义类型

#### 问题2：规范格式错误

**症状：**
- Markdown格式错误
- JSON格式不规范

**解决方案：**
1. 检查生成的规范文件语法
2. 使用Markdown验证工具
3. 手动修正格式错误

### 工具3：优化代码

#### 问题1：代码优化效果不佳

**症状：**
- 优化建议不相关
- 代码质量没有改善

**解决方案：**
1. 提供更详细的规范内容：
   ```json
   {
     "code": "原始代码",
     "spec": "# API规范\\n## 用户认证\\n- 输入验证：用户名必须3-20字符，密码至少8位\\n- 密码加密：使用bcrypt，salt rounds = 12\\n- 错误处理：返回统一的错误格式\\n- JWT Token：有效期24小时\\n- 安全检查：防止SQL注入和XSS攻击",
     "feedback": "需要添加详细的输入验证和错误处理"
   }
   ```

2. 给出具体的优化建议：
   ```json
   {
     "feedback": "请添加：1. 输入参数验证 2. 错误处理机制 3. 日志记录 4. 性能优化 5. 安全加固"
   }
   ```

#### 问题2：优化后的代码无法运行

**症状：**
- 语法错误
- 逻辑错误

**解决方案：**
1. 检查优化后的代码语法
2. 验证依赖项是否正确
3. 测试关键功能点
4. 逐步回滚优化内容

## 🔧 性能问题

### 问题1：MCP服务器响应慢

**症状：**
- 工具调用响应时间超过10秒
- 服务器CPU使用率高

**诊断步骤：**
1. 监控服务器性能：
   ```bash
   top
   ```

2. 查看内存使用：
   ```bash
   free -m
   ```

3. 检查日志文件大小：
   ```bash
   ls -lh logs/
   ```

**解决方案：**
1. 清理日志文件：
   ```bash
   > logs/server.log
   ```

2. 增加内存分配：
   ```bash
   export NODE_OPTIONS="--max-old-space-size=4096"
   ```

3. 重启服务器释放内存

### 问题2：大项目生成超时

**症状：**
- 大型项目生成失败
- 文件写入超时

**解决方案：**
1. 分批生成项目组件
2. 简化初始项目结构
3. 增加超时时间：
   ```javascript
   // 在配置文件中设置
   timeout: 60000 // 60秒
   ```

4. 使用增量生成模式

## 🔧 兼容性问题

### 问题1：Trae版本不兼容

**症状：**
- MCP工具在Trae中不显示
- 工具调用格式错误

**解决方案：**
1. 检查Trae版本要求
2. 更新Trae到最新版本
3. 检查MCP协议版本兼容性

### 问题2：操作系统兼容性

**症状：**
- Windows系统下路径问题
- macOS权限问题

**解决方案：**

**Windows特定问题：**
1. 使用双反斜杠：
   ```json
   {
     "projectPath": "C:\\Users\\Username\\Projects"
   }
   ```

2. 以管理员身份运行命令提示符

**macOS特定问题：**
1. 修复权限：
   ```bash
   sudo chown -R $(whoami) .
   ```

2. 关闭系统完整性保护（谨慎使用）

## 🔧 日志和调试

### 启用详细日志

**配置日志级别：**
```javascript
// config.js
module.exports = {
  logging: {
    level: 'debug',
    file: 'logs/server.log',
    console: true
  }
}
```

**查看实时日志：**
```bash
# 查看服务器日志
tail -f logs/server.log

# 查看错误日志
tail -f logs/error.log

# 查看工具调用日志
tail -f logs/tools.log
```

### 调试工具调用

**启用调试模式：**
```bash
DEBUG=mcp:* npm start
```

**查看工具调用详情：**
```bash
# 查看特定工具日志
grep "create_openspec_project" logs/server.log

# 查看错误详情
grep -A 5 -B 5 "Error" logs/server.log
```

## 🔧 常见错误代码

### 错误代码对照表

| 错误代码 | 描述 | 解决方案 |
|----------|------|----------|
| MCP001 | 连接超时 | 检查网络连接，重启服务器 |
| MCP002 | 参数验证失败 | 检查JSON格式和参数类型 |
| MCP003 | 文件系统错误 | 检查权限和磁盘空间 |
| MCP004 | 内存不足 | 增加内存或简化操作 |
| MCP005 | 权限被拒绝 | 检查文件和目录权限 |
| MCP006 | 项目已存在 | 删除现有项目或更改名称 |
| MCP007 | 模板加载失败 | 检查模板文件完整性 |
| MCP008 | 代码解析错误 | 检查代码语法和格式 |

### 错误处理最佳实践

1. **记录详细错误信息：**
   ```javascript
   try {
     // 工具逻辑
   } catch (error) {
     console.error('Tool error:', {
       message: error.message,
       stack: error.stack,
       timestamp: new Date().toISOString(),
       parameters: params
     });
   }
   ```

2. **提供用户友好的错误消息：**
   ```javascript
   return {
     success: false,
     error: {
       code: 'MCP002',
       message: '参数格式错误，请检查输入的JSON格式',
       suggestion: '使用JSON验证工具检查参数格式'
     }
   };
   ```

## 🔧 获取帮助

### 自助资源

1. **查看日志文件**：`logs/`目录下的详细日志
2. **检查配置文件**：确认配置参数正确
3. **验证依赖版本**：确保所有依赖版本兼容
4. **搜索已知问题**：查看GitHub Issues

### 社区支持

1. **GitHub Issues**：报告bug和功能请求
2. **技术文档**：查看最新的使用文档
3. **社区论坛**：与其他用户交流经验

### 联系支持

当以上方法都无法解决问题时：

1. 收集详细的错误信息
2. 记录复现步骤
3. 提供环境信息（操作系统、Node.js版本等）
4. 联系技术支持团队

---

💡 **提示**：大多数问题都可以通过查看日志文件和检查配置来解决！

🚀 **下一步**：查看 [ADVANCED.md](./ADVANCED.md) 了解高级配置和自定义选项！