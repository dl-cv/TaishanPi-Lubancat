# TaishanPi-Lubancat

泰山派 (TaishanPi-3M RK3576) 内核移植到鲁班猫3 (Lubancat3) SDK 的文档和补丁。

## 项目背景

- **硬件**：泰山派 TaishanPi-3M（RK3576），无 WiFi/BT 模块
- **目标**：在鲁班猫3 SDK 中编译泰山派内核，并能在泰山派硬件上正常启动
- **系统**：Debian 12 (Bookworm) Gnome

## 仓库内容

| 文件 | 说明 |
|------|------|
| `Rk3576泰山派移植鲁班猫.md` | 移植过程、修改清单、编译指令 |
| `泰山派鲁班猫差异.md` | 泰山派与鲁班猫的全面对比报告 |
| `uboot-emmc-timing.patch` | U-Boot eMMC 时序修改补丁 |
| `rkbin-ddr-frequency.patch` | rkbin DDR 频率修改补丁 |

## 代码提交状态

| 仓库 | 分支 | 状态 |
|------|------|------|
| [dl-cv/kernel](https://github.com/dl-cv/kernel) | `rk3576` | ✅ 已推送 |
| [LubanCat/u-boot](https://github.com/LubanCat/u-boot) | — | ❌ 无写权限，使用 patch |
| [LubanCat/rkbin](https://github.com/LubanCat/rkbin) | — | ❌ 无写权限，使用 patch |

## 快速开始

### 1. 克隆鲁班猫 SDK

```bash
git clone https://github.com/LubanCat/lubancat-linux-sdk.git
```

### 2. 应用内核修改

内核修改已推送到 [dl-cv/kernel](https://github.com/dl-cv/kernel) 的 `rk3576` 分支：

```bash
cd kernel-6.1
git remote add dl-cv https://github.com/dl-cv/kernel.git
git fetch dl-cv rk3576
git checkout dl-cv/rk3576 -b rk3576-tspi-port
```

### 3. 应用 U-Boot 补丁

```bash
cd u-boot
git apply ../TaishanPi-Lubancat/uboot-emmc-timing.patch
```

### 4. 应用 rkbin 补丁

```bash
cd rkbin
git apply ../TaishanPi-Lubancat/rkbin-ddr-frequency.patch
```

### 5. 复制 SDK defconfig 和 overlay

```bash
cp TaishanPi-Lubancat/tspi_3m_rk3576_debian_bookworm_gnome_defconfig device/rockchip/.chips/rk3576/
cp TaishanPi-Lubancat/boot_init.sh debian12/overlay/etc/init.d/
```

### 6. 编译

```bash
./build.sh tspi_3m_rk3576_debian_bookworm_gnome_defconfig
./build.sh all
```

## 完整编译烧录指令

详见 [Rk3576泰山派移植鲁班猫.md](./Rk3576泰山派移植鲁班猫.md)

## 差异对比

详见 [泰山派鲁班猫差异.md](./泰山派鲁班猫差异.md)
