# MikroTik RouterOS Skill

为 AI Agent 提供全面的 MikroTik RouterOS 官方文档引用、配置指导和连接方法，确保在回答 RouterOS 相关问题时使用权威和准确的信息。

## ✨ 功能特性

### 1. 官方文档优先
指导 AI Agent 优先查阅 MikroTik 官方文档：
- **RouterOS Manual** (https://help.mikrotik.com/docs/spaces/ROS/overview) - 官方配置参考
- **MikroTik Wiki** (https://wiki.mikrotik.com/) - 传统文档和社区指南
- **MikroTik Forum** - 社区讨论（非权威）

优先级：RouterOS Manual > Wiki > Forum

### 2. 完整的连接方法
包含三种主要连接方式：
- **SSH 连接**：配置、密钥认证、命令执行
- **API 连接**：官方协议、Python 库、端口配置
- **GUI 访问**：WinBox、WebFig

### 3. CLI 语法规范
提供 RouterOS 命令行语法规则：
- 命令结构：`/category/subcategory/action`
- 参数格式：`parameter=value`
- 脚本语法：变量、注释、控制流

### 4. 常用功能快速参考
涵盖主要配置模块：
- IP 地址配置
- 防火墙和 NAT
- DHCP 服务器
- 路由协议
- Bridge 和 VLAN
- 无线网络
- VPN（WireGuard, PPP）

## 📦 安装方法

### 方法 1: 手动安装

```bash
# 1. 创建 skill 目录
mkdir -p ~/.claude/skills/mikrotik-routeros

# 2. 下载 SKILL.md
curl -o ~/.claude/skills/mikrotik-routeros/SKILL.md \
  https://raw.githubusercontent.com/nichwang88/claude-skills/main/mikrotik-routeros/SKILL.md

# 3. 重启 Claude Code
```

### 方法 2: Git 克隆

```bash
# 1. 克隆整个仓库
git clone https://github.com/nichwang88/claude-skills.git

# 2. 复制 mikrotik-routeros skill
cp -r claude-skills/mikrotik-routeros ~/.claude/skills/

# 3. 重启 Claude Code
```

### 方法 3: 符号链接（推荐用于开发）

```bash
# 1. 克隆仓库到合适位置
cd ~/Projects
git clone https://github.com/nichwang88/claude-skills.git

# 2. 创建符号链接
ln -s ~/Projects/claude-skills/mikrotik-routeros ~/.claude/skills/mikrotik-routeros

# 3. 重启 Claude Code
```

## 🎯 使用场景

此 skill 会在以下场景自动触发：

### ✅ 应该使用
- MikroTik RouterOS 配置问题
- RouterOS CLI 命令语法
- 脚本编写
- 网络功能设置（VLAN、路由、防火墙等）
- SSH/API 连接方法
- RouterBOARD 硬件配置
- 故障排查

### ❌ 不应使用
- 通用网络概念（非 RouterOS 特定）
- 其他品牌路由器（Cisco、Ubiquiti 等）

## 📖 使用示例

### 示例 1: VLAN 配置

**用户提问：**
> 如何在 MikroTik RouterOS 中配置 VLAN？

**使用 skill 后的回答特点：**
- ✅ 引用 RouterOS 官方文档链接
- ✅ 提供 Bridge VLAN Filtering 推荐方法
- ✅ 包含完整的配置步骤和命令
- ✅ 说明版本要求
- ✅ 安全注意事项

### 示例 2: Python API 连接

**用户提问：**
> 如何使用 Python 连接 MikroTik RouterOS API？

**使用 skill 后的回答特点：**
- ✅ 先说明官方 API 协议（端口 8728/8729）
- ✅ 然后提供第三方库作为便捷方案
- ✅ 包含完整的代码示例
- ✅ 引用官方 API 文档
- ✅ 包含安全建议

### 示例 3: 未明确记录的功能

**用户提问：**
> RouterOS 的某个不常见功能支持什么特性？

**使用 skill 后的回答特点：**
- ✅ 明确说明是否在官方文档中记录
- ✅ 提供找到的官方信息
- ✅ 如果未记录，明确指出并建议联系官方

## 🧪 测试验证

此 skill 通过完整的 TDD 流程创建：

### RED 阶段 - 基线测试
测试了没有 skill 时的问题：
- ❌ Agent 不引用官方文档
- ❌ 可能使用过时信息
- ❌ 缺少版本要求说明
- ❌ 第三方库优先于官方方法

### GREEN 阶段 - Skill 验证
创建 skill 后测试验证：
- ✅ VLAN 配置：正确引用官方文档，提供最佳实践
- ✅ API 连接：先说明官方协议，再提供便捷库
- ✅ 包含版本要求和安全建议

### REFACTOR 阶段 - 压力测试
- ✅ Container 功能测试：详细说明限制和要求
- ✅ 正确引用官方文档链接
- ✅ 明确标注版本和硬件要求

## 🔧 工作原理

Skill 通过以下机制确保答案质量：

### 1. 文档查询优先级
```
1. 查询 RouterOS Manual（官方权威文档）
2. 参考 MikroTik Wiki（传统文档）
3. 必要时参考 Forum（社区经验）
4. 提供答案并附上文档引用
```

### 2. Red Flags 检测
Skill 会阻止以下行为：
- "RouterOS 应该像 Cisco 一样..."
- "我记得在论坛看到..."
- "基于其他路由器，应该..."
- "第三方教程建议..."
- "它类似 Linux，所以..."

### 3. 关键原则
1. **始终引用官方文档** - 不推测行为
2. **检查 RouterOS 版本** - 功能因版本而异
3. **引用 Manual URLs** - 包含具体文档链接
4. **验证 CLI 语法** - 使用官方命令参考
5. **安全优先** - 遵循 MikroTik 安全最佳实践

## 📊 效果对比

| 方面 | 没有 Skill | 使用 Skill |
|------|-----------|-----------|
| **文档来源** | 混合论坛、Wiki、个人博客 | ✅ 优先官方 Manual |
| **连接方法** | 只提供第三方库 | ✅ 官方协议 + 第三方库 |
| **CLI 语法** | 可能过时或错误 | ✅ 基于官方文档 |
| **版本要求** | 很少提及 | ✅ 明确标注版本 |
| **安全建议** | 不完整或缺失 | ✅ 遵循官方最佳实践 |
| **文档链接** | 很少或没有 | ✅ 始终包含链接 |

## 🤔 常见问题

### Q: Skill 会自动触发吗？
A: 是的。当用户提问涉及 MikroTik RouterOS 配置、CLI 命令、连接方法等内容时，Claude Code 会自动加载并遵循此 skill。

### Q: 可以手动调用吗？
A: 可以。使用命令 `/mikrotik-routeros` 手动加载。

### Q: 支持哪些 RouterOS 版本？
A: Skill 基于最新的 RouterOS 7.x 和 6.x 官方文档，会明确标注版本要求。

### Q: 包含 SwOS 文档吗？
A: 是的。Skill 包含 SwOS（用于 CRS 交换机）的相关参考。

### Q: 如何更新 Skill？
A: 如果使用 git clone 安装，运行 `git pull` 更新。如果手动安装，重新下载 SKILL.md 文件。

## 🔗 相关链接

### 官方资源
- [RouterOS Manual](https://help.mikrotik.com/docs/spaces/ROS/overview)
- [MikroTik Wiki](https://wiki.mikrotik.com/)
- [MikroTik Forum](https://forum.mikrotik.com/)
- [SSH 文档](https://help.mikrotik.com/docs/spaces/ROS/pages/132350014/SSH)
- [API 文档](https://help.mikrotik.com/docs/spaces/ROS/pages/47579160/API)

### 连接工具
- **SSH**: OpenSSH, PuTTY
- **API 库**: librouteros, routeros-api
- **GUI**: WinBox, WebFig

## 📝 技术细节

### Skill 元数据

```yaml
name: mikrotik-routeros
description: Use when answering questions about MikroTik RouterOS configuration,
  scripting, network setup, firewall rules, routing protocols, or connecting to
  RouterOS via SSH/API. Use when user mentions MikroTik, RouterOS, or RouterBOARD
  devices.
```

### 文件结构

```
mikrotik-routeros/
├── README.md    # 本文档（功能说明和安装指南）
└── SKILL.md     # Skill 定义文件（AI Agent 读取）
```

### 依赖工具

Skill 使用以下 Claude Code 工具：
- `WebFetch` - 获取官方文档内容
- `WebSearch` - 搜索特定配置说明
- `Bash` - SSH 连接示例

## 📄 许可证

MIT License

## 👤 作者

**Nicholas**
- GitHub: [@nichwang88](https://github.com/nichwang88)
- Email: nic.hwang@hotmail.com

## 🙏 致谢

- MikroTik 开发团队提供详细的官方文档
- Claude Code 团队提供 Skills 框架
- Superpowers Marketplace 提供 TDD 方法论指导
