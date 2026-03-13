# Mac Reminder Bridge (苹果提醒事项桥接器) 🔔

一个轻量级的 HTTP 桥接工具，允许运行在 Docker 容器（尤其是 OpenClaw 等 AI 智能体）中的程序，通过简单的 REST API 接口直接管理 macOS 原生的**提醒事项 (Reminders.app)**。

## 🌟 核心价值
由于 Docker 容器是沙箱隔离的，无法直接访问宿主机的系统硬件或 API。本项目通过在 Mac 宿主机上运行一个小型的 Flask 监听程序，为 AI 助手打开了一个“传送门”，使其能够直接帮你记笔记、设闹钟、同步待办事项到你的 iPhone/Mac 全家桶中。

## 🚀 主要功能
- **全功能管理**：支持提醒事项的增、删、改、查（CRUD）。
- **自然语言适配**：专为 AI Agent 的工具调用（Tool Calling）场景设计。
- **智能解析**：支持设定到期时间、优先级、备注信息以及指定列表分类。
- **安全保障**：
  - 支持 IP 白名单策略。
  - 支持共享密钥 (`X-Bridge-Secret`) 认证。
  - 内置 AppleScript 注入防护。
- **原生体验**：基于 AppleScript 实现，数据与你的 iCloud 账号完美同步。

## 📦 安装与配置

### 1. 宿主机端 (你的 Mac)
克隆本项目并安装依赖：
```bash
git clone https://github.com/your-username/mac-reminder-bridge.git
cd mac-reminder-bridge
pip install -r requirements.txt
```

启动监听服务：
```bash
python3 listener.py
```
*注意：首次运行时，macOS 会弹出窗口询问“是否允许终端访问提醒事项”，请点击“允许”。*

### 2. 客户端 (Docker/OpenClaw 内部)
如果你使用的是 OpenClaw 平台，直接安装技能即可：
```bash
clawhub install mac-reminder-bridge
```

## 🛠 接口示例

### 检查服务状态
```bash
curl http://host.docker.internal:5000/health
```

### 创建一个提醒
```bash
curl -X POST http://host.docker.internal:5000/add_reminder \
     -H "Content-Type: application/json" \
     -d '{
       "task": "下午6点去拿快递",
       "due": "2026-03-14 18:00",
       "priority": "high",
       "notes": "这是由 AI 助手自动同步的提醒"
     }'
```

## 📜 开源协议
基于 MIT 协议开源。欢迎提交 Issue 或 Pull Request！
