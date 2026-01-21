---
name: mikrotik-routeros
description: Use when answering questions about MikroTik RouterOS configuration, scripting, network setup, firewall rules, routing protocols, or connecting to RouterOS via SSH/API. Use when user mentions MikroTik, RouterOS, or RouterBOARD devices.
---

# MikroTik RouterOS Reference

## Overview

MikroTik RouterOS has **official documentation** as the authoritative source. Always consult official docs before inferring behavior or suggesting third-party solutions.

## Documentation Sources

| Source | URL | Use For |
|--------|-----|---------|
| **RouterOS Manual** | https://help.mikrotik.com/docs/spaces/ROS/overview | Official configuration reference, all RouterOS features |
| **MikroTik Wiki** | https://wiki.mikrotik.com/ | Legacy documentation, community guides, older versions |
| **MikroTik Forum** | https://forum.mikrotik.com/ | Community discussions, troubleshooting (not authoritative) |

**Priority**: RouterOS Manual > Wiki > Forum

## When to Use This Skill

Use this skill when:
- Configuring RouterOS features (VLAN, routing, firewall, etc.)
- Writing RouterOS scripts
- Connecting to RouterOS via SSH or API
- Troubleshooting RouterOS issues
- Questions about RouterOS CLI commands
- Setting up network protocols (OSPF, BGP, VLAN, etc.)

**Do NOT use when:**
- Question is about general networking concepts (not RouterOS-specific)
- User asks about other router brands (Cisco, Ubiquiti, etc.)

## Connection Methods

### 1. SSH Connection

**Official Documentation**: https://help.mikrotik.com/docs/spaces/ROS/pages/132350014/SSH

**Default Configuration**:
- Protocol: SSH v2
- Port: TCP 22 (default, can be changed)
- Status: Enabled by default

**Basic SSH Connection**:
```bash
# Standard SSH
ssh admin@192.168.88.1

# With specific port
ssh -p 2222 admin@192.168.88.1

# Execute single command (ssh-exec)
ssh admin@192.168.88.1 "/interface print"
```

**SSH Key Authentication**:
```bash
# On RouterOS, import public key
/user ssh-keys import user=admin public-key-file=id_rsa.pub

# Connect from client
ssh -i ~/.ssh/id_rsa admin@192.168.88.1
```

**Supported Key Types**: RSA, Ed25519, Ed25519-sk (PEM, PKCS#8, or OpenSSH format)

### 2. API Connection

**Official Documentation**: https://help.mikrotik.com/docs/spaces/ROS/pages/47579160/API

**Connection Details**:
- Standard API: TCP 8728
- Secure API (API-SSL): TCP 8729
- Protocol: Sentence-based communication

**Enable API Service**:
```routeros
/ip service
set api disabled=no
set api-ssl disabled=no certificate=auto
```

**API Command Structure**:
- Commands start with `/`, spaces replaced by `/`
- Attributes: `=name=value`
- Queries: `?` prefix (for print commands)
- Sentences terminated by zero-length word

**Python Connection (Official Protocol)**:
```python
# Using official API protocol
import socket

# Connect to API
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(('192.168.88.1', 8728))

# Login and execute commands
# (See official API documentation for full implementation)
```

**Python Libraries (Third-party)**:
```bash
# Popular libraries
pip install librouteros
pip install routeros-api
```

**Example with librouteros**:
```python
import librouteros

api = librouteros.connect(
    host='192.168.88.1',
    username='admin',
    password='password',
    port=8728  # or 8729 for SSL
)

# Execute commands
interfaces = api.path('/interface')
print(list(interfaces))

api.close()
```

### 3. WinBox / WebFig

**GUI Access** (not for automation):
- WinBox: Native Windows/Wine application
- WebFig: Web interface (HTTP/HTTPS)

## CLI Syntax Rules

**Official Reference**: https://help.mikrotik.com/docs/spaces/ROS/pages/328059/RouterOS

1. **Command Structure**: `/category/subcategory/action`
   ```routeros
   /interface ethernet print
   /ip address add address=192.168.1.1/24 interface=ether1
   ```

2. **Parameters**: `parameter=value`
   ```routeros
   /ip firewall filter add chain=forward action=accept protocol=tcp dst-port=80
   ```

3. **Comments**: Lines starting with `#`
   ```routeros
   # This is a comment
   /system identity set name=Router1
   ```

4. **Scripts**: Use `:` for variables, `;` for line continuation
   ```routeros
   :local myVar "value"
   :put $myVar
   ```

## Common Use Cases

### Quick Reference Table

| Task | Command Path | Documentation |
|------|--------------|---------------|
| **IP Address** | `/ip address` | https://help.mikrotik.com/docs/spaces/ROS/pages/103841891/IP+Address |
| **Firewall** | `/ip firewall filter` | https://help.mikrotik.com/docs/spaces/ROS/pages/56459276/Filter |
| **NAT** | `/ip firewall nat` | https://help.mikrotik.com/docs/spaces/ROS/pages/56459267/NAT |
| **DHCP Server** | `/ip dhcp-server` | https://help.mikrotik.com/docs/spaces/ROS/pages/26476614/DHCP |
| **Routing** | `/routing` | https://help.mikrotik.com/docs/spaces/ROS/pages/130711553/Routing |
| **Bridge/VLAN** | `/interface bridge` | https://help.mikrotik.com/docs/spaces/ROS/pages/103841808/Bridging+and+Switching |
| **Wireless** | `/interface wireless` | https://help.mikrotik.com/docs/spaces/ROS/pages/103841999/Wireless |
| **VPN** | `/interface wireguard`, `/ppp` | https://help.mikrotik.com/docs/spaces/ROS/pages/42205224/WireGuard |

## Key Principles

1. **Always reference official documentation** - Don't infer behavior
2. **Check RouterOS version** - Features vary by version
3. **Cite Manual URLs** - Include specific documentation links
4. **Verify CLI syntax** - Use official command reference
5. **Security first** - Follow MikroTik security best practices

## Red Flags - STOP and Check Official Docs

These thoughts mean you're about to violate the skill:
- "RouterOS probably works like Cisco..."
- "I remember from a forum post..."
- "Based on other routers, it should..."
- "A third-party tutorial suggested..."
- "It's similar to Linux, so..."

**All of these mean: Stop. Check RouterOS Manual. If not documented, say so.**

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using Cisco/Linux syntax | Check RouterOS CLI syntax in official docs |
| Mixing Wiki and Manual info | Manual takes priority (Wiki may be outdated) |
| Assuming features from other routers | Verify in RouterOS documentation |
| Using outdated forum solutions | Check Manual for current best practices |
| Skipping version requirements | Always note RouterOS version requirements |

## Answer Template

When answering RouterOS questions:

```markdown
[Clear explanation based on official documentation]

**Configuration Example:**
```routeros
[Actual RouterOS CLI commands]
```

**Official Reference:**
- Manual: [relevant Manual URL]
- Additional: [Wiki URL if applicable]

**Version Requirements:**
- RouterOS version: [if applicable]

**Notes:**
- [Security considerations]
- [Performance considerations]
- [Caveats or limitations]
```

## Version-Specific Features

Always check and mention version requirements:
- **RouterOS 7.x**: New features, breaking changes from 6.x
- **RouterOS 6.x**: Stable, long-term support
- **SwOS**: For CRS switches only

**Find version**: `/system resource print`

## Security Best Practices

**From Official Docs**:
1. Change default admin password
2. Disable unused services (`/ip service`)
3. Use SSH keys instead of passwords
4. Restrict API access by IP (`/ip service set api address=trusted-ip`)
5. Enable firewall rules
6. Keep RouterOS updated

**Security Documentation**: https://help.mikrotik.com/docs/spaces/ROS/pages/8978504/User

## Real-World Impact

**Without this skill:**
- Agents mix forum posts with official docs
- Outdated or incorrect CLI syntax
- Third-party libraries suggested without official API reference
- No version awareness

**With this skill:**
- Authoritative answers from RouterOS Manual
- Correct CLI syntax and commands
- Official API documentation first, libraries as alternatives
- Version requirements clearly stated
- Security best practices included
