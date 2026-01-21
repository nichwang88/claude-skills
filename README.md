# Nicholas 的 Claude Skills 集合

这是我的个人 Claude Code Skills 集合，包含各种提升 AI Agent 工作效率的技能。

## 📚 可用 Skills

### [Surge Reference](./surge-reference)
用于给 AI Agent 提供全面的 Surge 文档和知识库引用，确保回答 Surge 配置问题时使用官方权威文档。

- **功能**: 指导 AI 优先查阅 Surge 官方文档（Manual、Knowledge Base、Release Log）
- **适用场景**: Surge 代理配置、DNS 设置、规则语法、模块使用、故障排查
- **特点**: 严格的文档优先级、不推测未记录行为、规范的答案格式

[查看详细文档 →](./surge-reference/README.md)

### [MikroTik RouterOS](./mikrotik-routeros)
为 AI Agent 提供 MikroTik RouterOS 官方文档引用、配置指导和连接方法（SSH/API）。

- **功能**: 引用官方 RouterOS Manual、提供 CLI 语法规范、SSH/API 连接方法
- **适用场景**: RouterOS 配置、脚本编写、网络设置、防火墙规则、路由协议、设备连接
- **特点**: 官方文档优先、完整的连接方法、CLI 语法验证、版本要求说明

[查看详细文档 →](./mikrotik-routeros/README.md)

## 🚀 快速开始

### 安装单个 Skill

```bash
# 克隆仓库
git clone https://github.com/nichwang88/claude-skills.git

# 复制需要的 skill 到 Claude Code 的 skills 目录
cp -r claude-skills/surge-reference ~/.claude/skills/
```

### 安装所有 Skills

```bash
# 克隆仓库
git clone https://github.com/nichwang88/claude-skills.git

# 复制所有 skills
cp -r claude-skills/*/ ~/.claude/skills/
```

### 验证安装

启动 Claude Code 后，skill 会自动加载。您可以手动调用测试：

```bash
# 调用 surge-reference skill
/surge-reference
```

## 📖 Skill 目录结构

```
claude-skills/
├── README.md                    # 本文档
├── surge-reference/            # Surge 文档参考 skill
│   ├── README.md               # Skill 详细说明
│   └── SKILL.md                # Skill 定义文件
├── mikrotik-routeros/          # MikroTik RouterOS skill
│   ├── README.md               # Skill 详细说明
│   └── SKILL.md                # Skill 定义文件
└── [其他 skills...]
```

## 🛠 开发说明

### Skill 创建原则

所有 skills 遵循 TDD (Test-Driven Development) 方法论：

1. **RED 阶段**: 创建测试场景，记录没有 skill 时的基线行为
2. **GREEN 阶段**: 编写 minimal skill，测试验证有效性
3. **REFACTOR 阶段**: 识别漏洞并改进，确保 bulletproof

### Skill 文件规范

每个 skill 必须包含：
- `SKILL.md`: Skill 定义文件（必需）
- `README.md`: 功能说明和使用文档（推荐）
- 支持文件: 如需要可添加脚本、配置示例等

### SKILL.md 结构

```markdown
---
name: skill-name
description: Use when [触发条件和使用场景]
---

# Skill Name

## Overview
核心原则和简介

## When to Use
使用场景和触发条件

## [具体内容...]
```

## 🤝 贡献

这是我的个人 skills 集合，但欢迎提供建议和反馈！

如果您有改进建议：
1. Fork 本仓库
2. 创建 Feature 分支
3. 提交 Pull Request

## 📄 许可证

MIT License

## 👤 作者

**Nicholas**
- GitHub: [@nichwang88](https://github.com/nichwang88)
- Email: nic.hwang@hotmail.com

## 🔗 相关资源

- [Claude Code 官方文档](https://github.com/anthropics/claude-code)
- [Superpowers Skills Marketplace](https://github.com/ckreiling/superpowers-marketplace)
- [Surge 官方文档](https://manual.nssurge.com/)
- [MikroTik RouterOS 官方文档](https://help.mikrotik.com/docs/spaces/ROS/overview)
