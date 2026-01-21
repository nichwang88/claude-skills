# UniFi Network Management Skill

为 AI Agent 提供 Ubiquiti UniFi 网络设备管理的完整指导，包括 API 使用、SSH 命令、安全最佳实践和故障排除。

## 📖 Skill 概述

这个 skill 为 AI Agent 提供管理 UniFi 网络设备所需的全面知识，确保：
- **版本感知的 API 使用** - 根据 Controller 版本使用正确的 API 端点
- **安全优先** - 正确处理凭据、SSL 证书和认证
- **备份保护** - 在破坏性操作前强制备份
- **全面的 SSH 指导** - 包含设备认证、常用命令和故障排除

## 🎯 适用场景

### API 管理
- 连接 UniFi Controller API（任意版本）
- 自动化网络配置和设备管理
- 获取设备状态、客户端信息、网络统计
- 症状：`UniFi API returns 404`、`API endpoint not found`、`CSRF token missing`

### SSH 设备管理
- SSH 连接到 UniFi 设备（AP、交换机、网关、Console）
- 执行设备配置和诊断命令
- 设备采纳和重新配置
- 症状：`SSH authentication failed`、`device won't adopt`、`connection refused`

### 故障排除
- 设备离线或无法连接
- API 认证失败
- SSL 证书问题
- 配置错误恢复

## ✨ 核心功能

### 1. 版本感知的 API 使用

```python
# ❌ 错误：假设端点路径
url = "https://controller:8443/api/s/default/stat/device"

# ✅ 正确：根据版本选择端点
if controller_version >= 7:
    url = "https://controller:8443/proxy/network/api/s/default/stat/device"
else:
    url = "https://controller:8443/api/s/default/stat/device"
```

### 2. 安全凭据管理

```python
# ❌ 错误：硬编码凭据
username = "admin"
password = "password123"

# ✅ 正确：环境变量
import os
username = os.getenv("UNIFI_USER")
password = os.getenv("UNIFI_PASS")
```

### 3. 破坏性操作前备份

```bash
# ❌ 错误：直接重置
ssh root@device "syswrapper.sh restore-default"

# ✅ 正确：先备份
ssh root@device "cat /tmp/system.cfg" > backup-$(date +%Y%m%d).cfg
# 验证备份
ls -lh backup-*
# 然后重置
ssh root@device "syswrapper.sh restore-default"
```

### 4. 完整的 SSH 指导

- 如何从 Controller 获取设备 SSH 凭据
- 设备类型的认证差异（Console vs AP/Switch）
- SSH 密钥认证配置
- 常用诊断命令

## 📚 包含的参考资料

### API 端点快速参考
- Login/Logout 端点（多版本）
- 设备列表、客户端列表、系统信息
- 版本差异说明

### SSH 命令参考
- 设备信息和状态检查
- Controller 连接管理
- 工厂重置和重启
- 日志查看和诊断

### 故障排除流程图
- 系统化的诊断步骤
- 常见问题和解决方法
- 诊断命令清单

## 🚨 解决的常见错误

### 错误 1: API 404 错误
**原因**: 使用了错误版本的 API 端点路径

**解决**:
- 检查 Controller 版本
- 查看内置 API 文档（Settings > Control Plane > Integrations）
- 使用版本感知的端点选择

### 错误 2: CSRF Token 缺失
**原因**: UniFi Network 7.2+ 需要 CSRF token

**解决**:
```python
response = session.post(login_url, json=credentials)
if "X-CSRF-Token" in response.headers:
    session.headers["X-CSRF-Token"] = response.headers["X-CSRF-Token"]
```

### 错误 3: SSH 认证失败
**原因**: 使用了错误的凭据

**解决**:
- 从 Controller 获取设备 SSH 密码：Settings > System > Device Authentication
- 区分 Console（root）和设备（controller 管理）的认证
- 使用 SSH 密钥认证

### 错误 4: 工厂重置后配置丢失
**原因**: 没有备份就执行重置

**解决**:
- 总是先备份配置文件
- 验证备份文件存在
- 保存多个备份副本

### 错误 5: API 会话限制
**原因**: 没有正确关闭 API 会话

**解决**:
```python
# 使用 try-finally 确保清理
try:
    devices = client.get_devices()
finally:
    client.logout()
```

## 📦 安装

```bash
# 克隆仓库
git clone https://github.com/nichwang88/claude-skills.git

# 复制到 Claude Code skills 目录
cp -r claude-skills/unifi-network-management ~/.claude/skills/
```

## 🔍 使用示例

启动 Claude Code 后，当你提出 UniFi 相关问题时，skill 会自动加载：

```
User: "我需要通过 API 获取所有 UniFi 设备的状态"
Claude: [加载 unifi-network-management skill]
Claude: 首先，我需要知道你的 UniFi Controller 版本...
```

## 📖 官方文档资源

Skill 引用以下官方和社区资源：

- [UniFi Help Center](https://help.ui.com/hc/en-us/categories/6583256751383-UniFi) - 官方文档
- [Getting Started with UniFi API](https://help.ui.com/hc/en-us/articles/30076656117655) - 官方 API 指南
- [Ubiquiti Community Wiki](https://ubntwiki.com/products/software/unifi-controller/api) - 社区 API 文档
- [UniFi SSH Commands](https://lazyadmin.nl/home-network/unifi-ssh-commands/) - SSH 命令指南
- [Art-of-WiFi UniFi API Client](https://github.com/Art-of-WiFi/UniFi-API-client) - 社区 API 客户端

## 🛠 开发方法

本 skill 遵循 TDD (Test-Driven Development) 方法创建：

### RED 阶段 - 基线测试
运行了 3 个压力场景，记录了没有 skill 时 AI Agent 的行为：
1. **时间 + 权威压力**: 快速 API + SSH 访问
2. **沉没成本 + 疲劳**: 工作 2 小时后的工厂重置请求
3. **权威 + 不完整信息**: 5 分钟内完成 API 脚本

发现的问题：
- 硬编码凭据和禁用 SSL 验证
- 假设 API 端点路径（不验证版本）
- 缺少官方文档引用
- SSH 认证混乱
- 没有备份提醒

### GREEN 阶段 - Skill 编写
编写 skill 解决所有基线测试中发现的问题：
- 安全最佳实践章节
- 版本感知的 API 使用
- 完整的 SSH 认证指导
- 备份工作流
- 官方文档资源链接

### REFACTOR 阶段 - 完善改进
- 添加常见错误和解决方法
- 创建故障排除流程图
- 提供诊断命令清单
- 补充真实世界影响说明

## 🎯 实际效果

**使用前**:
- API 脚本在 Controller 升级后失效（错误的端点）
- 安全问题（硬编码凭据、禁用 SSL）
- 意外工厂重置导致配置丢失
- SSH 认证失败（错误的凭据）

**使用后**:
- 版本感知的 API 使用（先检查文档）
- 安全的凭据管理（环境变量、SSH 密钥）
- 修改前备份工作流（无数据丢失）
- 正确的 SSH 认证（controller 管理的凭据）

## 🤝 贡献

欢迎提供反馈和改进建议！如有问题或建议，请提交 Issue 或 Pull Request。

## 📄 许可证

MIT License

## 👤 作者

**Nicholas**
- GitHub: [@nichwang88](https://github.com/nichwang88)
- Email: nic.hwang@hotmail.com

## 🔗 相关 Skills

- [MikroTik RouterOS](../mikrotik-routeros) - 路由器配置管理
- [Surge Reference](../surge-reference) - 代理服务器配置

---

**更新日期**: 2026-01-21
**Skill 版本**: 1.0.0
