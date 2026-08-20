# OpenWrt Toolchain Builder — Configuration Guide

## How to customize toolchain configs

Each target config file in `config/` controls what gets compiled into the toolchain.

### Key settings

| Setting | Options | Description |
|---------|---------|-------------|
| `CONFIG_MAKE_TOOLCHAIN` | y/n | Must be `y` to build toolchain |
| `CONFIG_GCC_VERSION_11` | y | Use GCC 11.x |
| `CONFIG_GCC_VERSION_13` | y | Use GCC 13.x (if available) |
| `CONFIG_BINUTILS_VERSION_2_37` | y | Binutils version |
| `CONFIG_LIBC_USE_GLIBC` | y | Use glibc (more compatible, larger) |
| `CONFIG_LIBC_USE_MUSL` | y | Use musl (smaller, faster boot) |
| `CONFIG_TARGET_KERNEL_PARTSIZE` | number | Kernel partition size (MB) |
| `CONFIG_TARGET_ROOTFS_PARTSIZE` | number | Rootfs partition size (MB) |
| `CONFIG_KERNEL_BUILD_USER` | string | Builder name (cosmetic) |
| `CONFIG_KERNEL_BUILD_DOMAIN` | string | Builder domain (cosmetic) |

### glibc vs musl

| | glibc | musl |
|---|---|---|
| Size | Larger (~2MB) | Smaller (~400KB) |
| Compatibility | Better (more apps work) | Some apps may fail |
| Speed | Standard | Slightly faster boot |
| Use case | x86, Raspberry Pi | ARM routers, embedded |

### Adding custom packages

Edit the workflow file and add after feeds install:

```yaml
- name: Add custom packages
  run: |
    cd ${{ env.BUILD_ROOT }}/openwrt
    # Clone extra packages
    git clone --depth=1 https://github.com/xiaorouji/openwrt-passwall package/passwall
    # Install
    ./scripts/feeds install -a
```