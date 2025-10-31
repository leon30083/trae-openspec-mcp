# 发布到 NPM 指南

## 为什么发布到 NPM？
- 安装更便捷：用户无需克隆仓库，直接 `npx trae-openspec-mcp` 或 `npm i -g`。
- 版本管理：通过语义化版本（SemVer）清晰标注变更，便于升级和回滚。
- 分发标准化：统一的发布渠道，易于被其他工具与脚手架集成。
- 信任与可见性：公开包提升可见度，便于社区使用与反馈。

## 发布前准备
- 确保 `package.json` 完整：
  - `name`: `trae-openspec-mcp`
  - `version`: 遵循语义化版本，如 `0.1.0`
  - `bin`: 提供可执行入口，例如：
    ```json
    {
      "bin": {
        "trae-openspec-mcp": "mcp-server.js"
      }
    }
    ```
  - `main`: 入口文件（如需要）
  - `files`: 限定发布内容（可选）
  - `license`: 开源协议（如 MIT）
  - `repository`, `bugs`, `homepage`: 指向 GitHub 仓库
- 入口文件兼容性：`mcp-server.js` 顶部添加 `#!/usr/bin/env node` 以支持全局执行（Windows 下可选）。
- 质量保障：运行 `npm test` 或烟囱测试脚本确保功能正常。

## 发布流程
1. 登录 NPM（一次性）
   ```bash
   npm login
   ```
2. 版本更新（遵循 SemVer）
   ```bash
   # 补丁版本（修复）
   npm version patch
   # 次版本（新增功能）
   npm version minor
   # 主版本（不兼容变更）
   npm version major
   ```
3. 发布到 NPM
   ```bash
   npm publish --access public
   ```
4. 验证发布
   ```bash
   npm info trae-openspec-mcp
   npx trae-openspec-mcp --help
   ```

## 使用方式（发布后）
- 本地执行：
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
- 或全局安装后：
  ```bash
  npm i -g trae-openspec-mcp
  trae-openspec-mcp
  ```

## 注意事项
- 私有包需 `npm publish --access restricted`。
- 若包名被占用，可使用作用域：`@your-scope/trae-openspec-mcp`。
- 避免将大文件或敏感信息发布到 NPM（使用 `.npmignore` 或 `files` 字段）。
- Windows 行尾警告（LF/CRLF）不影响功能，但建议统一设置 `.gitattributes`。

## 常见问题
- 登录失败：检查 NPM 源和网络代理。
- 发布失败：确认版本号未被使用、无未提交更改、权限足够。
- 可执行权限：在 *nix 系统确保入口文件有执行权限：`chmod +x mcp-server.js`。