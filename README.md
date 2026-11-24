# netboot.xyz (LazyCat 喵喵商店)

把几十种操作系统的安装器汇总成一个网络启动（PXE）菜单：局域网里的电脑开机时选择从网络启动，就能直接挑选并安装系统，无需制作 U 盘。

## 来源

- 上游：https://netboot.xyz （基于 iPXE 的网络安装器项目，Apache-2.0）
- 镜像：上游官方容器镜像 `ghcr.io/netbootxyz/netbootxyz`，本仓库直接镜像跟踪 `latest`（镜像站 `ghcr.1ms.run/netbootxyz/netbootxyz`），不存储上游代码、不做源码构建
- 打包：`ca-x/lazycat-github-action`，镜像 digest 钉住校验，每日自动检查上游镜像更新并发布新版本

## 引导原理

客户端启动时经历两段：

1. **TFTP (UDP 69)**：从本机拉 iPXE 引导文件（`netboot.xyz.kpxe` / `netboot.xyz.efi` / `netboot.xyz-arm64.efi`）
2. **HTTP (TCP 80)**：引导文件就绪后，从 `live_endpoint` 指向的地址加载菜单与系统镜像

DHCP 需由现有服务器/路由器承担：把 next-server 指向懒猫主机 IP，并按客户端类型设置引导文件名。

## 使用说明

见 LPK 内 `usage`（安装后可在应用详情查看）：首次启动下载菜单、DHCP 配置步骤、本地资产缓存加速。
