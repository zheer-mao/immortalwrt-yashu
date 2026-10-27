# ImmortalWrt 纯净编译：京东云 亚瑟 AX1800 Pro

本仓库用 GitHub Actions 自动编译 ImmortalWrt，机型是京东云无线宝 亚瑟 AX1800 Pro（`jdcloud_re-ss-01`，高通 IPQ60xx）。

这是给朋友用的纯净官方版：只选设备 + LuCI，其他全部用官方默认。没有 argon 主题、adblock、nginx、中文包等额外东西，刷完是英文界面。

源码用 ImmortalWrt master 分支，已包含 odhcpd IPv6 漏洞（CVE-2026-53921）的修复。

## 仓库里有什么

- `.config`：编译配置。目标平台 qualcommax / ipq60xx / jdcloud_re-ss-01，只勾了 LuCI，其他用官方默认。
- `.github/workflows/build.yml`：GitHub Actions 工作流。跑在 ubuntu-22.04 上，自动拉源码、更新 feeds、加载 `.config`、编译、上传成品到 Artifacts 和 Releases。

## 怎么编译

1. 打开仓库的 **Actions** 页面，点左侧的 **Build ImmortalWrt for 亚瑟 AX1800 Pro**。
2. 点 **Run workflow**，再点绿色的 **Run workflow** 按钮。
3. 等它跑完（约 1.5 小时），在当次运行页面的 **Artifacts** 里下载 `immortalwrt-jdcloud_re-ss-01`，或在 **Releases** 里下载，找这个文件：
   - `immortalwrt-qualcommax-ipq60xx-jdcloud_re-ss-01-squashfs-sysupgrade.bin`：正式固件

改了 `.config` 再推到 main 分支也会自动触发一次编译。

## 刷机

- 在现系统 LuCI 里：系统 → 备份与更新 → 上传 sysupgrade.bin，**不要勾选保留配置**（不同系统/不同配置，保留会出问题）。
- 刷完网关是 192.168.1.1，首次登录没有密码（登录时设一个），电脑要重拿 IP（子网变了）。
- 刷完是英文界面，要中文的话，WAN 通后 SSH 里跑：`apk add luci-i18n-base-zh-cn luci-i18n-firewall-zh-cn luci-i18n-package-manager-zh-cn`，装完在系统 → 语言与风格里选中文。

## 注意

- 设备行必须用横杠原样（jdcloud_re-ss-01），改成下划线会被 defconfig 静默丢弃，默认编出 8devices_mango-dvk（不要刷错设备的文件）。
- 这是公开仓库，**不要往里面写宽带账号、WiFi 密码等敏感信息**。
