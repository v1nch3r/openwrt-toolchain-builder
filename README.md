# OpenWrt Toolchain Builder

Automated OpenWrt/LEDE toolchain builder using GitHub Actions CI/CD.

Build pre-compiled toolchains for multiple targets — download, mount, and start compiling firmware in minutes instead of hours.

## 🎯 Features

- **Multi-target support** — x86_64, ARM64, ARM, Raspberry Pi, and more
- **Pre-compiled toolchain images** — skip 3+ hour compile time
- **Overlay-ready** — mount read-only, overlay for customizations
- **GitHub Actions** — free builds on public repos, no local PC needed
- **Auto-release** — toolchains uploaded to GitHub Releases automatically
- **Modern base** — uses latest coolsnowwolf/lede master branch
- **Clean & documented** — every step explained, easy to customize

## 📦 Supported Targets

| Target | Config | Description |
|--------|--------|-------------|
| `x86_64` | `config/x86_64.conf` | PC, firewall appliance, mini PC |
| `armvirt64` | `config/armvirt64.conf` | ARM virtual (QEMU, Amlogic S9xxx) |
| `bcm27xx` | `config/bcm27xx.conf` | Raspberry Pi 4 (bcm2711) |
| `bcm27xx_bcm2709` | `config/bcm2709.conf` | Raspberry Pi 2/3 |
| `rockchip_armv8` | `config/rockchip.conf` | NanoPi R2S/R4S, Orange Pi |

## 🚀 Usage

### Build a toolchain

1. Go to **Actions** tab
2. Select **Build Toolchain** workflow
3. Click **Run workflow**
4. Choose target and options
5. Wait 1-3 hours
6. Download toolchain image from **Releases**

### Use the toolchain

```bash
# Download toolchain image
wget https://github.com/USERNAME/openwrt-toolchain-builder/releases/download/toolchain-x86_64/toolchain-x86_64.img.gz
gunzip toolchain-x86_64.img.gz

# Mount and build
mkdir -p openwrt-ro openwrt-overlay workdir
mount -o loop toolchain-x86_64.img openwrt-ro
mount -t overlay overlay -o lowerdir=openwrt-ro,upperdir=openwrt-overlay,workdir=workdir openwrt

cd openwrt
# Add your packages, configs, etc.
make defconfig
make -j$(nproc)
```

## 🔧 How It Works

```
GitHub Actions (Ubuntu 22.04, 2-core, 7GB RAM)
  │
  ├── 1. Free disk space (remove .NET, Android, Haskell)
  ├── 2. Install build dependencies
  ├── 3. Clone OpenWrt source (coolsnowwolf/lede)
  ├── 4. Update & install feeds
  ├── 5. Apply target config
  ├── 6. Compile toolchain (make toolchain/install)
  ├── 7. Pack into .img + .tar.gz
  └── 8. Upload to GitHub Release
```

## ⚙️ Configuration

Edit `config/<target>.conf` to customize:
- Kernel version
- Package selection
- Filesystem support
- USB/network drivers

## 📜 License

MIT — free to use, modify, and distribute.

## 👤 Author

Isaac (Hermes Agent) — built for Vincher 🤙