# Claude Code 工具集

这是一个用于收集和维护 Claude Code 相关工具的仓库。随着时间推移，这里会不断添加各种提升 Claude Code 使用体验的实用工具。

## 项目结构

本仓库使用 Git Submodules 方式管理各个独立的工具项目，每个工具都保持独立的版本控制和开发流程。

## 工具列表

### 1. Claude Code Image Paste (WSL)

**路径**: `claude-code-image-paste-wsl/`
**仓库**: https://github.com/git-qiaozq/claude-code-image-paste-wsl

一个专为 Claude Code 优化的 VS Code 扩展，支持在终端中快速粘贴剪贴板图片。

**核心特性**:
- 📋 支持剪贴板截图直接粘贴
- 📁 支持从文件管理器复制图片文件
- 🔄 WSL 路径自动转换
- 📂 自动保存到项目目录
- 🧹 自动清理旧图片（保留最新 N 张）
- 🤖 自动添加 `@` 前缀（Claude Code 文件导入格式）
- 📝 自动更新 `.gitignore`

**适用环境**:
- VS Code / Cursor / Windsurf / VSCodium 等所有基于 VS Code 的 IDE
- Windows 10/11 + WSL2 环境

**快速使用**:
```bash
# 在 VS Code 终端打开时，按下快捷键
Ctrl+Alt+V
```

**详细文档**: [查看 README](./claude-code-image-paste-wsl/README.md)

---

### 2. Claude Image Paste

**路径**: `claude-image-paste/`
**仓库**: https://github.com/aggroot/claude-image-paste

原版的 Claude 图片粘贴扩展，提供基础的剪贴板图片粘贴功能。

**核心特性**:
- 📋 剪贴板图片粘贴
- 📁 支持多种图片格式
- 🔄 WSL 路径转换
- ✏️ 交互式文件重命名
- 📂 自定义保存目录

**详细文档**: [查看 README](./claude-image-paste/README.md)

---

## 安装与使用

### 克隆仓库（包含所有子模块）

```bash
# 克隆主仓库及所有子模块
git clone --recursive https://github.com/git-qiaozq/claude-code-utils.git

# 如果已经克隆了主仓库，但没有子模块内容
git submodule update --init --recursive
```

### 更新所有工具到最新版本

```bash
# 更新所有子模块到最新提交
git submodule update --remote --merge
```

### 单独使用某个工具

每个工具都可以独立使用，直接进入对应的子模块目录查看其文档即可。

## 推荐配置

如果你使用 `claude-code-image-paste-wsl`，建议在 VS Code/Cursor 的 `settings.json` 中添加：

```json
{
  "claudeImagePaste.saveDirectory": ".claude-images",
  "claudeImagePaste.skipRenamePrompt": true,
  "claudeImagePaste.maxImages": 10
}
```

这样可以：
- ✅ 将图片保存到项目的 `.claude-images/` 目录
- ✅ 跳过重命名提示，加快工作流
- ✅ 自动清理旧图片，保留最新 10 张

## 贡献指南

欢迎提交新的 Claude Code 相关工具！

### 添加新工具

1. Fork 本仓库
2. 将你的工具仓库作为 submodule 添加：
   ```bash
   git submodule add <你的工具仓库URL> <工具目录名>
   ```
3. 更新本 README，添加工具说明
4. 提交 Pull Request

### 报告问题

- 对于特定工具的问题，请到对应工具的仓库提交 Issue
- 对于本仓库组织结构的建议，请在本仓库提交 Issue

## 许可证

本仓库采用 MIT 许可证。各子模块工具遵循各自的许可证，详见各工具目录。

## 致谢

感谢所有为 Claude Code 生态系统做出贡献的开发者！

---

**最后更新**: 2026-01-04
**维护者**: [qiaozq](https://github.com/git-qiaozq)