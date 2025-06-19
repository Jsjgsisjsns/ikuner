## 前言

本指南旨在帮助您通过购买海外 VPS 服务器并自行搭建代理节点，实现在中国大陆地区稳定、安全、高性价比地访问国际互联网（俗称“科学上网”）。我们将涵盖 VPS 的选择、购买、基础设置、代理协议的选择与部署，以及客户端的配置。

**重要提示：**

*   请遵守您所在地区以及服务器所在地区的法律法规。
*   自行搭建和使用代理服务具有一定的技术门槛和潜在风险，请务必谨慎操作。
*   本指南提供的信息基于当前（截至 2025 年 5 月）的普遍认知和实践，网络环境和技术可能随时发生变化。

## 目录

1.  选择合适的 VPS 提供商和套餐
2.  购买 VPS 及初始设置
3.  选择并部署代理协议/软件 (推荐 Xray)
4.  服务器安全加固
5.  客户端配置
6.  常见问题与维护

---

## 1. 选择合适的 VPS 提供商和套餐

在之前的研究中，我们已经比较了几家适合中国大陆访问的 VPS 提供商，并分析了不同的代理协议。综合考虑性价比、稳定性、速度和易用性，以下是一些建议：

*   **线路选择：** 优先选择针对中国大陆优化的线路，如 CN2 GIA、AS9929、CMI 等。亚太地区（如香港、日本、韩国、新加坡、美国西海岸洛杉矶）的机房通常延迟较低。
*   **推荐提供商（基于前期研究）：**
    *   **搬瓦工 (BandwagonHost):** 提供高质量的 CN2 GIA 线路，稳定性好，配置较高，但价格相对较贵。适合追求稳定性和性能的用户。注意其隐私政策提到会收集使用数据（含 IP）。
    *   **GigsGigsCloud:** 提供 CN2 GIA 和其他优化线路，价格相对较低，但配置可能略低，且其使用条款明确禁止公共代理服务（个人使用通常可以，但需注意）。适合预算有限的用户。
    *   **DMIT:** 提供高质量的 CN2 GIA 和 CMI 线路，性能和稳定性都不错，价格较高。
    *   **HostDare:** 提供 CN2 GIA/GT 线路，价格适中，有时提供大硬盘套餐。
*   **套餐选择：**
    *   **配置：** 对于个人科学上网，通常入门级配置（如 1 核 CPU, 512MB-1GB RAM, 10-20GB SSD 硬盘, 500GB-1TB 月流量）即可满足需求。如果需要多人共享或有其他用途，可以适当提高配置。
    *   **带宽：** 1Gbps 带宽是目前主流 VPS 的标配，实际速度受线路质量影响更大。
    *   **流量：** 根据个人使用习惯选择，一般 500GB-1TB/月足够。
    *   **价格：** 年付通常比月付更优惠。关注提供商的促销活动（如黑色星期五、双十一等）可以获得更好的价格。

**最终选择建议：**

*   **追求稳定和性能:** 优先考虑搬瓦工或 DMIT 的 CN2 GIA 套餐。
*   **追求性价比:** 可以考虑 GigsGigsCloud 或 HostDare 的优化线路套餐，或在促销期间购买搬瓦工的普通线路套餐。

---

## 2. 购买 VPS 及初始设置

购买流程大同小异，通常包括注册账户、选择套餐、配置选项、付款等步骤。

1.  **访问提供商官网:** 通过我们之前搜索到的链接或直接搜索提供商名称访问官网。
2.  **注册账户:** 按照提示填写邮箱、密码等信息完成注册，可能需要邮箱验证。
3.  **选择套餐:** 浏览 VPS 产品页面，根据第 1 部分的建议选择合适的机房、线路和配置套餐。
4.  **配置选项:**
    *   **付款周期:** 选择月付、季付、年付等。
    *   **操作系统:** 选择一个你熟悉的 Linux 发行版，推荐 **Ubuntu 20.04/22.04 LTS** 或 **Debian 10/11**，因为它们拥有活跃的社区支持和丰富的文档。避免选择 CentOS 7（即将停止维护）或更新的 CentOS Stream 版本（滚动更新可能不稳定）。
    *   **其他选项:** 如备份、额外 IP 等，按需选择。
5.  **使用优惠码 (如有):** 在结算页面查找输入优惠码的地方，输入可用的优惠码获取折扣。
6.  **选择付款方式:** 通常支持信用卡、PayPal、支付宝等。选择方便的方式完成付款。
7.  **等待 VPS 开通:** 付款成功后，VPS 通常会在几分钟到几小时内开通。你会收到包含 VPS IP 地址、root 密码、SSH 端口等信息的邮件。

**初始设置 (通过 SSH 连接):**

你需要一个 SSH 客户端来连接你的 VPS。Windows 用户可以使用 PuTTY, Xshell, MobaXterm 等；macOS 和 Linux 用户可以直接使用终端 (Terminal)。

1.  **打开 SSH 客户端:**
2.  **连接服务器:**
    *   **主机/IP 地址:** 输入你的 VPS IP 地址。
    *   **端口:** 输入 SSH 端口 (默认为 22，如果提供商指定了其他端口，请使用指定端口)。
    *   **用户名:** 通常是 `root`。
    *   **密码:** 输入你的 root 密码 (输入时可能不显示字符)。
    ```bash
    # 示例命令 (macOS/Linux 终端)
    ssh root@YOUR_VPS_IP -p YOUR_SSH_PORT
    ```
3.  **首次连接确认:** 首次连接时会提示确认服务器的指纹 (fingerprint)，输入 `yes` 并回车。
4.  **修改 root 密码 (强烈建议):** 登录成功后，立即修改默认的 root 密码。
    ```bash
    passwd
    ```
    按照提示输入当前密码和两次新密码。请设置一个强密码！
5.  **更新系统软件包:** 保持系统最新是安全的基础。
    ```bash
    # Ubuntu/Debian
    apt update && apt upgrade -y

    # 如果是 CentOS (不推荐新购)
    # yum update -y
    # yum upgrade -y
    ```
6.  **创建新用户 (可选但推荐):** 不建议直接使用 root 用户进行日常操作。
    ```bash
    adduser your_username # 替换 your_username 为你想要的用户名
    # 设置密码并按提示填写信息

    # 赋予 sudo 权限 (Ubuntu/Debian)
    usermod -aG sudo your_username

    # 赋予 sudo 权限 (CentOS)
    # usermod -aG wheel your_username
    ```
    之后可以退出 root 登录，使用新用户通过 SSH 重新登录。

至此，VPS 的购买和基础设置完成。接下来我们将选择并部署代理协议。

---


## 3. 选择并部署代理协议/软件 (推荐 Xray)

基于之前的研究，Xray (使用 VLESS + XTLS/TLS 或 VLESS + TCP + TLS 协议) 是目前在性能、安全性和抗封锁性方面综合表现较好的选择之一。Trojan-Go 也是一个优秀的选择，原理类似。本指南将以 Xray 为例进行说明。

**部署方式：**

为了简化部署过程，我们推荐使用社区广泛使用的一键安装脚本。这些脚本通常会自动处理依赖安装、配置文件生成、服务管理等问题。请注意，使用第三方脚本存在一定安全风险，请选择信誉良好、代码开源的脚本。

**推荐脚本：**

目前比较流行的 Xray 安装脚本是 `Xray-install` (由 `XTLS` 社区维护)。

**安装步骤 (以 `Xray-install` 脚本为例):**

1.  **连接到你的 VPS:** 使用 SSH 客户端登录你的 VPS (建议使用非 root 用户配合 sudo)。

2.  **下载并执行安装脚本:**
    ```bash
    # 确保已安装 curl 和 sudo (Ubuntu/Debian 通常自带)
    # sudo apt update
    # sudo apt install curl -y

    # 执行官方安装脚本 (推荐)
    bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
    ```
    *   脚本会自动检测你的系统并安装最新稳定版的 Xray-core。
    *   安装完成后，Xray 的主程序通常位于 `/usr/local/bin/xray`，配置文件位于 `/usr/local/etc/xray/config.json`，并已设置为开机自启。

3.  **配置 Xray (核心步骤):**
    这是最关键的一步。你需要编辑 Xray 的配置文件 `/usr/local/etc/xray/config.json` 来设置服务器端的代理参数。以下是一个 VLESS + TCP + TLS 的示例配置，你需要根据实际情况修改。

    **准备工作:**
    *   **域名:** 你需要一个域名，并将其解析到你的 VPS IP 地址。免费域名或付费域名均可。确保 DNS 解析已生效。
    *   **SSL 证书:** 为了使用 TLS 加密，你需要一个 SSL 证书。可以通过脚本自动申请 Let's Encrypt 免费证书，或者使用自己已有的证书。
    *   **安装 Caddy/Nginx (可选但推荐):** 使用 Caddy 或 Nginx 作为反向代理和 Web 服务器，可以更好地伪装流量，并自动处理 TLS 证书申请和续期。Caddy 配置更简单。

    **安装 Caddy (推荐):**
    ```bash
    # 参考 Caddy 官方文档安装最新版本
    # 例如在 Ubuntu/Debian 上:
    sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
    sudo apt update
    sudo apt install caddy
    ```

    SSH 的保护
    # 可以通过编辑 /etc/fail2ban/jail.local 文件进行自定义配置
    sudo systemctl status fail2ban # 查看状态
    ```
*   **启用 TCP BBR (可选，优化性能):** BBR 是 Google 开发的 TCP 拥塞控制算法，可以显著提高网络吞吐量和降低延迟，尤其是在跨国链路上。
    ```bash
    # 检查内核版本 (需要 4.9 或更高版本)
    uname -r

    # 编辑 sysctl 配置
    sudo nano /etc/sysctl.conf

    # 在文件末尾添加以下两行
    net.core.default_qdisc=fq
    net.ipv4.tcp_congestion_control=bbr

    # 保存并退出，然后应用配置
    sudo sysctl -p

    # 验证 BBR 是否启用
    sysctl net.ipv4.tcp_congestion_control # 应输出 bbr
    lsmod | grep bbr # 应看到 tcp_bbr 模块
    ```
*   **定期检查日志:** 关注系统日志 (`/var/log/auth.log`, `/var/log/syslog` 等) 和 Xray 的日志，及时发现异常活动。
*   **数据备份:** 定期备份重要数据和配置文件。

完成这些安全加固措施后，你的 VPS 会相对更安全。

---