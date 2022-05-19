
#### EUserv IPV6添加WARP IPV4，脚本主要针对OpenVZ、LXC架构的IPV6 only VPS，默认已设置SSH下IPV4优先！

#### 原先详细视频教程及探讨：https://youtu.be/78dZgYFS-Qo

#### 最新德鸡EUserv抛弃DNS64、自定义IP分流教程（推荐）：https://youtu.be/fY9HDLJ7mnM

-------------------------------------------------------------------------------------------------------

##### 一：恢复EUserv官方DNS64（重装系统者，可直接跳到第二步脚本安装）
```
echo -e "search blue.kundencontroller.de\noptions rotate\nnameserver 2a02:180:6:5::1c\nnameserver 2a02:180:6:5::4\nnameserver 2a02:180:6:5::1e\nnameserver 2a02:180:6:5::1d" > /etc/resolv.conf
```

##### 二、重装系统能解决99%的问题！无须添加DNS64！Debian 10/Ubuntu 20.04系统脚本（有无成功可查看脚本末尾提示）
```
wget https://cdn.jsdelivr.net/gh/hu-min/EUserv-addv4-warp/warp4.sh && chmod +x warp4.sh && ./warp4.sh
```

##### 三、配置文件wgcf.conf和注册文件wgcf-account.toml都已备份在/etc/wireguard下！

------------------------------------------------------------------------------------------------------------- 
##### 分流配置文件(以下默认全局IPV4优先，详情见视频教程)
```
{ 
"outbounds": [
    {
      "tag":"IP6-out",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag":"IP4-out",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIPv4" 
      }
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "IP4-out",
        "domain": [""] 
      },
      {
        "type": "field",
        "outboundTag": "IP6-out",
        "network": "udp,tcp" 
      }
    ]
  }
}
``` 

 ---------------------------------------------------------------------------------------------------------

#### 相关WARP进程命令

手动临时关闭WARP网络接口
```
wg-quick down wgcf
```
手动开启WARP网络接口 
```
wg-quick up wgcf
```

查看WARP当前统计状态
```
wg
```

启动systemctl enable wg-quick@wgcf

开始systemctl start wg-quick@wgcf

重启systemctl restart wg-quick@wgcf

停止systemctl stop wg-quick@wgcf

关闭systemctl disable wg-quick@wgcf

#### 下期彩蛋：EUserv双栈warp？可以有！

---------------------------------------------------------------------------------------------------------------------

感谢P3terx大及原创者们，参考来源：
 
https://p3terx.com/archives/debian-linux-vps-server-wireguard-installation-tutorial.html

https://p3terx.com/archives/use-cloudflare-warp-to-add-extra-ipv4-or-ipv6-network-support-to-vps-servers-for-free.html

https://luotianyi.vc/5252.html
