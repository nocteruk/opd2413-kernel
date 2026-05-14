# OPD2413 Kernel Build

## 设备信息
- **型号**: 一加平板2 Pro (OPD2413)
- **SoC**: 骁龙8至尊版 (SM8750)
- **系统**: ColorOS 16 (OPD2413-16.0.3.501) → Android 16
- **内核版本**: 6.6.89 (GKI)
- **Root**: SukiSU (API 40762 / KernelSU v4.1.x)

## 黑屏四重不兼容修复

### Fix #1: header_version 4 → 5 (Android 16 init_boot)
Android 16 使用 init_boot 分区格式，header_version 必须为 5。

### Fix #2: 随机 AVB 密钥 → 设备特定密钥 (ColorOS 16)
ColorOS 16 严格校验 AVB 签名，使用随机密钥会导致黑屏。

### Fix #3: 补丁冲突 (SUSFS + SukiSU + kernel_patches)
按正确顺序应用：SUSFS → SukiSU → kernel_patches，确保显示驱动配置不被覆盖。

### Fix #4: 手机分区布局 → 平板分区布局 (init_boot)
平板 (OPD2413) 的 init_boot 分区布局与手机不同，需要适配。

## 构建流程
1. 基于 WildKernels/OnePlus_KernelSU_SUSFS 构建
2. 使用清华镜像源同步 AOSP 内核源码
3. 应用 OPD2413 设备特定配置 (OP-PAD-2-PRO_Config)
4. 修复上述四个黑屏问题
5. 编译生成 kernelImage + dtbo + boot.img

## 使用方法
在 GitHub 仓库页面手动触发 Workflow Dispatch，选择 kernel_version 和 branch。
