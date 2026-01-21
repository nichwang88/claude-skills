# Surge Reference Skill

为 AI Agent 提供全面的 Surge 文档和知识库引用，确保在回答 Surge 配置相关问题时使用官方权威文档。

## ✨ 功能特性

### 1. 官方文档优先
指导 AI Agent 优先查阅三个官方文档源：
- **Surge Manual** (https://manual.nssurge.com/) - 权威规范和配置参考
- **Knowledge Base** (https://kb.nssurge.com/) - 实用指南和 FAQ
- **Release Log** - 最新 beta 更新和行为变更

### 2. 清晰的文档优先级
当文档来源冲突时，按照以下优先级：
```
Release Log > Surge Manual > Knowledge Base
```

### 3. 不推测未记录的行为
- 如果功能未在官方文档中记录，明确指出"未记录"
- 避免混入博客、论坛等非官方来源
- 防止基于相似功能的推测

### 4. 规范的答案格式
提供的答案包含：
- 基于官方文档的清晰解释
- 实际可用的配置示例
- 官方文档链接引用
- 版本要求和注意事项

## 📦 安装方法

### 方法 1: 手动安装

```bash
# 1. 创建 skill 目录
mkdir -p ~/.claude/skills/surge-reference

# 2. 下载 SKILL.md
curl -o ~/.claude/skills/surge-reference/SKILL.md \
  https://raw.githubusercontent.com/nichwang88/claude-skills/main/surge-reference/SKILL.md

# 3. 重启 Claude Code
```

### 方法 2: Git 克隆

```bash
# 1. 克隆整个仓库
git clone https://github.com/nichwang88/claude-skills.git

# 2. 复制 surge-reference skill
cp -r claude-skills/surge-reference ~/.claude/skills/

# 3. 重启 Claude Code
```

### 方法 3: 符号链接（推荐用于开发）

```bash
# 1. 克隆仓库到合适位置
cd ~/Projects
git clone https://github.com/nichwang88/claude-skills.git

# 2. 创建符号链接
ln -s ~/Projects/claude-skills/surge-reference ~/.claude/skills/surge-reference

# 3. 重启 Claude Code
```

## 🎯 使用场景

此 skill 会在以下场景自动触发：

### ✅ 应该使用
- 回答 Surge 配置选项问题
- 解释 Surge 规则语法或行为
- Surge 代理问题故障排查
- 关于 DNS、模块、脚本或策略的问题
- 验证功能可用性或版本要求

### ❌ 不应使用
- 通用网络概念问题（非 Surge 特定）
- 询问其他代理工具（Clash、V2Ray 等）

## 📖 使用示例

### 示例 1: DNS over HTTPS 配置

**用户提问：**
> 如何在 Surge 中配置 DNS over HTTPS (DoH)？

**使用 skill 后的回答：**
```markdown
# Surge中配置DNS over HTTPS (DoH)

## 基本语法

在 `[General]` 部分配置全局加密DNS服务器：

```ini
[General]
encrypted-dns-server = https://1.1.1.1/dns-query, https://8.8.8.8/dns-query
```

## 官方参考文档

- Manual: https://manual.nssurge.com/dns/doh.html
- KB: https://kb.nssurge.com/surge-knowledge-base/technotes/dns
```

### 示例 2: 未记录功能的处理

**用户提问：**
> Surge的`subnet`参数支持同时配置IPv4和IPv6吗？

**使用 skill 后的回答：**
```markdown
**重要结论：未在官方文档中明确记录。**

通过查阅Surge Manual和Knowledge Base，我发现官方文档没有明确说明`ROUTER:`参数是否支持同时配置IPv4和IPv6地址。

建议：
1. 实际测试验证
2. 联系Surge官方支持获取确认
3. 查看最新Release Log

**官方参考：**
- https://manual.nssurge.com/rule/subnet.html
```

## 🧪 测试验证

此 skill 通过完整的 TDD 流程创建：

### RED 阶段 - 基线测试
测试了没有 skill 时的问题：
- Agent 混合使用博客、社区讨论等非官方来源
- 缺少文档引用和版本信息
- 可能推测未记录的行为

### GREEN 阶段 - Skill 验证
创建 skill 后测试验证：
- ✅ 正确引用 Manual 和 Knowledge Base
- ✅ 遵循文档优先级
- ✅ 提供规范的配置示例和引用

### REFACTOR 阶段 - 压力测试
- ✅ 对未记录功能明确指出"未记录"
- ✅ 不推测或混用非官方来源
- ✅ 遵循"Red Flags"规则

## 🔧 工作原理

Skill 通过以下机制确保答案质量：

### 1. 文档查询工作流
```
1. 首先查询 Surge Manual（权威规范）
2. 查询 Knowledge Base（实用指南）
3. 如相关，查询 Release Log（最新变更）
4. 提供答案并附上文档引用
5. 如未找到，明确说明"未记录"
```

### 2. Red Flags 检测
Skill 会阻止以下行为：
- "基于类似功能，它可能..."
- "Surge应该像其他代理工具一样..."
- "我记得在博客中看到..."
- "社区讨论表明..."
- "它应该这样工作..."

### 3. 关键原则
1. **不推测未记录的行为** - 如果未记录，明确说明
2. **验证文档时效性** - 检查 Release Log 的最新变更
3. **引用来源** - 始终包含 Manual/KB 链接
4. **Manual 优先** - 从权威规范开始，再看实用指南

## 📊 效果对比

| 方面 | 没有 Skill | 使用 Skill |
|------|-----------|-----------|
| **文档来源** | 混合博客、论坛、官方文档 | ✅ 仅官方文档 |
| **答案准确性** | 可能过时或不准确 | ✅ 基于最新官方规范 |
| **文档引用** | 很少或没有 | ✅ 始终包含链接 |
| **未记录功能** | 可能推测 | ✅ 明确标注"未记录" |
| **优先级冲突** | 不清楚以哪个为准 | ✅ 清晰的优先级 |

## 🤔 常见问题

### Q: Skill 会自动触发吗？
A: 是的。当用户提问涉及 Surge 配置、DNS、规则语法等内容时，Claude Code 会自动加载并遵循此 skill。

### Q: 可以手动调用吗？
A: 可以。使用命令 `/surge-reference` 手动加载。

### Q: Skill 文件很大吗？
A: 不大。SKILL.md 只有约 600 词，对 token 占用很小。

### Q: 如何更新 Skill？
A: 如果使用 git clone 安装，运行 `git pull` 更新。如果手动安装，重新下载 SKILL.md 文件。

### Q: 能用于其他代理工具吗？
A: 不能。此 skill 专为 Surge 设计。其他工具需要创建对应的 skill。

## 📝 技术细节

### Skill 元数据

```yaml
name: surge-reference
description: Use when answering questions about Surge proxy configuration,
  DNS settings, rule syntax, module usage, or troubleshooting Surge-related
  issues. Use when user mentions Surge, surge.conf, or network proxy
  configuration.
```

### 文件结构

```
surge-reference/
├── README.md    # 本文档（功能说明和安装指南）
└── SKILL.md     # Skill 定义文件（AI Agent 读取）
```

### 依赖工具

Skill 使用以下 Claude Code 工具：
- `WebFetch` - 获取官方文档内容
- `WebSearch` - 搜索特定配置说明（如需要）

## 🔗 相关链接

- [Surge 官方手册](https://manual.nssurge.com/)
- [Surge 知识库](https://kb.nssurge.com/)
- [Surge 官方网站](https://nssurge.com/)
- [Claude Code Skills 文档](https://github.com/anthropics/claude-code)

## 📄 许可证

MIT License

## 👤 作者

**Nicholas**
- GitHub: [@nichwang88](https://github.com/nichwang88)
- Email: nic.hwang@hotmail.com

## 🙏 致谢

- Surge 开发团队提供优秀的文档
- Claude Code 团队提供 Skills 框架
- Superpowers Marketplace 提供 TDD 方法论指导
