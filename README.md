# 跳板机

首先先有台国外主机, 能够通过ssh public key连接, 假设ssh登录地址是 `ssh -p 9022 dev@tunnel.chentm.com`

Mac下使用autossh自动连接ssh, 断线自动重连, 使用ssh tunnel作为socks5代理

先安装homebrew: 

    sudo ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
用homebrew安装autossh:

    `sudo brew install autossh`
    
用LaunchAgent设为开机自动启动 `sudo vi ~/Library/LaunchAgents/com.chentm.autossh.plist`

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    <key>Label</key>
    <string>com.chentm.autossh</string>
    <key>ProgramArguments</key>
    <array>
    <string>/usr/local/bin/autossh</string>
    <string>-M</string>
    <string>0</string>
    <string>-D</string>
    <string>9999</string>
    <string>-p</string>
    <string>9022</string>
    <string>root@chentm.com</string>
    <string>-N</string>
    </array>
    <key>KeepAlive</key>
    <false/>
    <key>RunAtLoad</key>
    <true/>
    </dict>
    </plist>

设为开机自启动:
   
    sudo launchctl load com.chentm.autossh.plist
    
立马启动

    sudo launchctl enable com.chentm.autossh.plist
    
此时autossh应该在工作了

    ps -ef | grep autossh
    
# 安装Chrome浏览器插件选择性让流量走代理

安装[SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList), 跟着他的教程

设置代理服务器部分, 设置为socks5代理, 地址是127.0.0.1, 端口9999

