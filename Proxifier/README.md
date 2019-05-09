# Proxifier

Proxifier 是一款功能非常强大的 socks5 客户端，可以让不支持通过代理服务器工作的网络程序能通过 HTTPS 或 SOCKS 代理或代理链。

- **Version**：3.42
- **Platform**：Windows XP, 7, 8, 10 **/** Server 2003, 2008, 2012
- **Official website**：[link](http://www.proxifier.com)
- **Keys**：[link](https://onhax.me/proxifier-keys)
- **Download**：[3.42](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/download/ProxifierSetup.exe)

## 0x01 Install

![proxifier_0001](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0001.png)

![proxifier_0002](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0002.png)

## 0x02 Register

- **Registration Keys for Standard Version**：

> KFZUS-F3JGV-T95Y7-BXGAS-5NHHP  
> T3ZWQ-P2738-3FJWS-YE7HT-6NA3K  
> KFZUS-F3JGV-T95Y7-BXGAS-5NHHP  
> 65Z2L-P36BY-YWJYC-TMJZL-YDZ2S  
> SFZHH-2Y246-Z483L-EU92B-LNYUA  
> GSZVS-5W4WA-T9F2E-L3XUX-68473  
> FTZ8A-R3CP8-AVHYW-KKRMQ-SYDLS  
> Q3ZWN-QWLZG-32G22-SCJXZ-9B5S4  
> DAZPH-G39D3-R4QY7-9PVAY-VQ6BU  
> KLZ5G-X37YY-65ZYN-EUSV7-WPPBS  
> 6JZUY-32TKX-TK9W7-DU387-9RWKZ  

- **Registration Keys for Portable Version**：

> 2TCKX-TYQHL-NFN33-3YEDY-QW65D  
  
![proxifier_0003](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0003.png)

## 0x03 Configure

**需要配置三个部分**：

- **Proxy Server**
- **Proxification Rule**
- **Name Resolution**

**Step 1**：输入代理服务器的ip和端口（默认 1080），然后选择 SHOCKS Versin 5

![proxifier_0004](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0004.PNG)

**Step 2**：此配置防止本地循环代理，如果此配置存在，就不用再配置

![proxifier_0007](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0007.PNG)

![proxifier_0005](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0005.PNG)

**Step 3**：此配置是通过代理服务器解析域名

![proxifier_0006](https://github.com/GHlyanin/Environmental-construction/blob/master/Proxifier/image/proxifier_0006.PNG)

## 0x04 Others

- 以上设置是默认 Proxifier 为全局代理
- 如果某应用不需要走全局代理，只需要添加规则，配置该应用 Proxification Rule 为直连
- 如果减少代理流量，只需要设置 Proxification Rule 为直连，然后配置需要走代理的应用为 SOCKS Version 5
