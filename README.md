# 跳板机

首先先有台国外主机, 能够通过ssh public key连接, 假设ssh登录地址是 `ssh -p 9022 dev@tunnel.chentm.com`

Mac下使用autossh自动连接ssh, 断线自动重连, 使用ssh tunnel作为socks5代理

先安装homebrew: 

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
注意，如果安装过程中断网或者其他情况导致安装失败，使用下面命令重新安装

    rm -rf /usr/local/Cellar /usr/local/.git
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
用homebrew安装autossh:

    `brew install autossh`
    
用LaunchAgent设为开机自动启动（文件内容根据具体情况进行修改） `vi ~/Library/LaunchAgents/com.chentm.autossh.plist`

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
   
    launchctl load com.chentm.autossh.plist（此处应该是完整的文件路径）
    
此时如果配置没有问题，autossh应该在工作了，查看一下进程

    ps -ef | grep autossh
    
# 安装Chrome浏览器插件选择性让流量走代理

设置代理服务器部分, 设置为socks5代理, 地址是127.0.0.1, 端口9999

我安装SwitchyOmega中有一步需要更新一个配置，设置成sock5时候，更新不下来，改成sock4可以更新（sock4和sock5的区别，sock4只能代理TCP连接，sock5可以代理更多地连接，比如UDP）

安装[SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega/wiki/GFWList), 跟着他的教程

