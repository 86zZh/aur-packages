# tencent-wechat

> 腾讯微信（WeChat）Linux 原生客户端 AUR 打包工程与极简安全防护运行时。  
> 专为现代 Linux 桌面（Wayland、Niri、Hyprland、GNOME、KDE Plasma 6）深度调优。

[![AUR package](https://img.shields.io/aur/version/tencent-wechat)](https://aur.archlinux.org/packages/tencent-wechat)
[![Update Check](https://github.com/qwerprog/tencent-wechat/actions/workflows/auto-update.yml/badge.svg)](https://github.com/qwerprog/tencent-wechat/actions)
[![License: Proprietary](https://img.shields.io/badge/License-Proprietary-blue.svg)](https://www.wechat.com/us/service_terms.html)

---

## ⚠️ 免责声明 / Disclaimer

**中文**：  
本项目为社区开源维护的非官方 Arch Linux 打包构建工程。WeChat、微信 及相关商标、Logo、版权均归**腾讯科技（深圳）有限公司（Tencent Holdings Limited）**所有。本项目仅作为 Linux 平台的兼容性与体验优化辅助工具，代码本身仅包含打包与启动脚本，不包含、不修改任何微信核心闭源二进制代码，不以牟利为目的，与腾讯官方无任何形式的隶属或合作关系。

**English**:  
This project is an unofficial community packaging effort for Arch Linux. WeChat and associated trademarks, logos, and copyrights belong to **Tencent Holdings Limited**. This repository only contains packaging and launcher scripts for compatibility and usability improvements on Linux. It does not contain or modify any proprietary binary code of WeChat, has no commercial intent, and is not affiliated with or endorsed by Tencent.

---

## ✨ 核心特性

- 🚀 **原生 Wayland 体验**：智能探测 Wayland 会话，自动配置 `text-input-unstable-v3`，完美解决 Fcitx5 等输入法候选窗不跟随、缩放模糊与浮动黑边问题。
- 📂 **拖拽发送 100% 修复**：直通宿主真实 `$HOME` 路径，从文件管理器或桌面随意拖拽文件/图片至聊天框秒发，彻底告别旧版沙盒“文件不存在”的痛点。
- 🛡️ **极简安全防护（Minimalist Bubblewrap Sandbox）**：
  - 采用轻量 `bwrap` 运行时，不造“虚拟假家目录”；
  - 核心隐私屏蔽：使用 `--tmpfs` 内存遮蔽 `~/.ssh` 和 `~/.gnupg`，防止商业软件窥探服务器私钥与密钥库；
  - 进程隔离：`--unshare-pid` 独立进程命名空间，微信无法探查宿主机其他软件进程；
  - 系统只读：根系统目录只读挂载，防止非预期修改。
- 💾 **历史记录安全防护**：启动时自动维护数据路径软链接，无缝平滑继承现有聊天记录，防止多份数据库分裂。
- 🧹 **纯净命名**：彻底告别冗长的 `universal` 历史包袱，系统命令规范为 `/usr/bin/wechat`，桌面启动项统一为 `wechat.desktop`。
- 🤖 **全自动监听更新**：集成 GitHub Actions 自动化流水线，定时检测腾讯官方 CDN 更新，自动更新并部署至 AUR。

---

## 📦 安装方式

### 方式一：使用 AUR 助手（推荐）

```bash
# 使用 yay
yay -S tencent-wechat

# 或使用 paru
paru -S tencent-wechat
```

### 方式二：手动构建安装

```bash
git clone https://aur.archlinux.org/tencent-wechat.git
cd tencent-wechat
makepkg -si
```

---

## 🛠️ 架构与启动逻辑

启动脚本 `/usr/bin/wechat` 的执行流程如下：

```text
[启动 wechat]
      │
      ├──> 检测 WAYLAND_DISPLAY
      │      ├──> 若存在: 启用 wayland 渲染模式 + text-input-v3 (解决输入法跟随)
      │      └──> 若不存在: 回退至 xcb (X11)
      │
      ├──> 校验聊天记录路径
      │      └──> 确保 ~/Documents/xwechat_files 与主力数据软链连接
      │
      └──> 启动极简 Bubblewrap 沙盒
             ├──> 直通真实 $HOME (保证拖拽/另存为全通)
             ├──> 遮蔽 ~/.ssh 与 ~/.gnupg (空 tmpfs 覆盖)
             ├──> 独立 PID 空间 + 系统只读
             └──> 启动官方二进制 /opt/wechat/wechat
```

---

## 🤝 参与贡献与致谢

欢迎提交 Issue 和 Pull Request 来优化适配体验！
感谢 Arch Linux 社区与所有致力于改善 Linux 中文桌面体验的开发者。
