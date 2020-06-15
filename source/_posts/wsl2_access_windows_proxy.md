---
title: WSL2 Access Windows Proxy
abbrlink: 47923
tags:
  - Proxy
  - Linux
categories:
  - WSL2
date: 2020-05-30 09:08:00
---
## IP设置

IP要使用主机局域网IP，例如192.168.123.176。127.0.0.1不行。再者，就是host.deocker.internal在安装docker的时候进行了重定向，所以使用这个也是可以的。
<!-- more -->

## 使用方法

1. 使用```proxychains```。安装就是```sudo apt install proxychains```，然后再进去```/etc/proxychains.conf```进行ip设置，使用直接在命令前面加proxychains。![image-20200606150558949](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/proxychains.conf.png)![image-20200606102952207](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/wsl proxy1.png)

   这个方法可能会出现dns无法解析的问题，要装依赖tor,使用`sudo apt install tor`即可。

2. 使用修改`.bashrc`, 在`.bashrc`中写代理和取消代理函数, 使用直接输入proxy、noproxy即可。![image-20200606151542147](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/proxy.bashrc.png)

```\# >>> proxy setting >>>
proxy () {
        export ALL_PROXY="socks5://192.168.123.176:10808"
        export all_proxy="socks5://192.168.123.176:10808"
        echo -e "Acquire::http::Proxy \"http://192.168.123.176:10809\";" | sudo tee -a /etc/apt/apt.c
onf > /dev/null
        echo -e "Acquire::https::Proxy \"http://192.168.123.176:10809\";" | sudo tee -a /etc/apt/apt.
conf > /dev/null
        curl myip.ipip.net
        }
```

```\# <<< proxy setting <<<
noproxy () {
        unset ALL_PROXY
        unset all_proxy
        sudo sed -i -e '/Acquire::http::Proxy/d' /etc/apt/apt.conf
        sudo sed -i -e '/Acquire::https::Proxy/d' /etc/apt/apt.conf
        curl myip.ipip.net
        }
```

**效果图：**

![image-20200606151800543](https://gitee.com/Rsweater_admin/Blog_Images/raw/master/img/proxy_.bashrc.png)

3. 就是使用`alias`将`export http_proxy=socks5://192.168.123.176:10808`做成段命令，直接在需要代理命令之前加别名即可。