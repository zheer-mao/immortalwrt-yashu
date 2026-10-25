# ImmortalWrt 编译：京东云 亚瑟 AX1800 Pro

本仓库用 GitHub Actions 自动编译 ImmortalWrt，机型是京东云无线宝 亚瑟 AX1800 Pro（`jdcloud_re-ss-01`，高通 IPQ60xx）。

源码用 ImmortalWrt master 分支，已包含 odhcpd IPv6 漏洞（CVE-2026-53921）的修复，编出来的固件不用再担心这个漏洞。

## 仓库里有什么

- `.config`：编译配置。目标平台 Qualcommax / IPQ60xx / jdcloud_re_ss_01，已勾选 LuCI 网页界面及中文语言包、防火墙页面、软件包页面、dnsmasq-full。
- `.github/workflows/build.yml`：GitHub Actions 工作流。跑在 ubuntu-22.04 上，自动拉源码、更新 feeds、加载 `.config`、编译、上传成品。

## 怎么编译

1. 打开仓库的 **Actions** 页面，点左侧的 **Build ImmortalWrt for 亚瑟 AX1800 Pro**。
2. 点 **Run workflow**，再点绿色的 **Run workflow** 按钮。
3. 等它跑完。首次编译一般要 2~4 小时，中途可以在 Actions 页面点进去看进度和日志。
4. 跑完后在当次运行页面的 **Artifacts** 里下载 `immortalwrt-jdcloud_re-ss-01`，解压后在 `bin/targets/qualcommax/ipq60xx/` 里找这两个文件：
   - `immortalwrt-qualcommax-ipq60xx-jdcloud_re-ss-01-squashfs-sysupgrade.bin`：正式固件
   - `immortalwrt-qualcommax-ipq60xx-jdcloud_re-ss-01-initramfs-uImage.itb`：内存启动用的临时系统

改了 `.config` 再推到 main 分支也会自动触发一次编译。

## 刷机注意（重要）

- **不要用不死 uboot 的网页界面刷这个固件**：自编译和官方快照都没有 factory 包，那个界面只认 factory 包，会失败。
- 正确路线：先刷好不死 uboot，进 uboot 控制台用 TFTP 引导 initramfs 进内存启动临时系统，再在临时系统里用 sysupgrade 写入闪存，**不要勾保留配置**。
- 刷机前先备份 ART 分区（WiFi 校准数据），TTL 线必须用 1.8V 的。

## 注意

这是公开仓库，**不要往里面写宽带账号、WiFi 密码等敏感信息**。`.config` 里只有编译选项，没有任何账号密码。
