# aur-packages

Personal Arch User Repository (AUR) packages collection maintained by **Chapman** ([@qwerprog](https://github.com/qwerprog)).

This monorepo houses PKGBUILDs and runtime wrappers for packages maintained on the Arch User Repository, with automated daily upstream checks, native Wayland optimization, and secure sandboxing.

---

## 📦 Packages Overview

| Package | Version | AUR Link | Features & Highlights |
| :--- | :--- | :--- | :--- |
| **[tencent-wechat](./tencent-wechat)** | ![AUR version](https://img.shields.io/aur/version/tencent-wechat?color=blue) | [AUR](https://aur.archlinux.org/packages/tencent-wechat) | • Native Wayland auto-detection & Fcitx5 `text-input-v3`<br>• Bubblewrap sandbox with real `$HOME` (fixes drag-and-drop file sending)<br>• Privacy protection (masks `~/.ssh` and `~/.gnupg`)<br>• Chat history preservation links |
| **[tencent-qq](./tencent-qq)** | ![AUR version](https://img.shields.io/aur/version/tencent-qq?color=blue) | [AUR](https://aur.archlinux.org/packages/tencent-qq) | • Native Ozone Wayland & `text-input-v3` IME cursor tracking<br>• Lightweight privacy sandbox with key masking<br>• Automated cleanup of vulnerable bundled `libssh2.so.1`<br>• Clean `/usr/bin/qq` and `/usr/bin/tencent-qq` commands |

---

## 🚀 Installation

Install using your preferred AUR helper (e.g. `paru` or `yay`):

```bash
# Tencent WeChat
paru -S tencent-wechat
# or
yay -S tencent-wechat

# Tencent QQ
paru -S tencent-qq
# or
yay -S tencent-qq
```

---

## 📂 Repository Layout

```text
aur-packages/
├── .github/workflows/
│   ├── sync-tencent-wechat.yml   # Daily upstream check & auto-push to AUR for WeChat
│   └── sync-tencent-qq.yml       # Upstream check & auto-push to AUR for QQ
│
├── tencent-wechat/               # WeChat AUR package files
│   ├── PKGBUILD
│   ├── .SRCINFO
│   ├── wechat.sh                 # Native Wayland & sandbox launcher
│   ├── wechat.desktop            # Clean desktop entry
│   └── LICENSE
│
└── tencent-qq/                   # QQ AUR package files
    ├── PKGBUILD
    ├── .SRCINFO
    ├── qq.sh                     # Native Wayland & sandbox launcher
    ├── qq.desktop
    └── LICENSE
```

---

## 🤖 Automation & CI/CD

Each package folder is independently tested and synced to its respective AUR Git repository via GitHub Actions:
- Upstream releases are checked daily at 02:00 UTC.
- When Tencent publishes a new package, the CI workflow automatically:
  1. Downloads and inspects the official debian package metadata;
  2. Bumps `pkgver` and calculates cryptographic `sha256` checksums;
  3. Updates `PKGBUILD` and regenerates `.SRCINFO`;
  4. Commits changes back to this GitHub monorepo;
  5. Authenticates and pushes directly to `ssh://aur@aur.archlinux.org/<pkgname>.git`.

---

## ⚖️ Disclaimer & License

- Packages here repackage official proprietary binaries distributed by Tencent. All copyrights belong to Tencent Technology (Shenzhen) Co., Ltd.
- The packaging scripts, wrappers, and configuration files are provided under the MIT / GPL compatible community licenses.
