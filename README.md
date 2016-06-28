# Finalspeed 1.2

FinalSpeed服务端Linux版,支持Centos,Ubuntu,Debian

### 注意问题
   1.服务端会启动iptables,如果服务器修改过ssh端口,请先开放ssh端口,否则可能导致ssh连接失败.
#### 开放端口命令
     service iptables start
     iptables -I INPUT -p tcp --dport 端口号 -j ACCEPT
     iptables -I OUTPUT -p tcp --sport 端口号 -j ACCEPT
     service iptables save
2.不熟悉不要乱改配置,如果无法连接,请卸载后一键安装,不要做任何修改,按照教程操作.

### 一键安装
    rm -f install_fs.sh
    wget  https://raw.githubusercontent.com/zhuanshicong/finalspeed/master/install_fs.sh
    chmod +x install_fs.sh
    ./install_fs.sh 2>&1 | tee install.log

debian,ubuntu下如果执行脚本出错,请切换到dash,
切换方法: sudo dpkg-reconfigure dash 选no

安装完后查看日志
tail -f /fs/server.log
如果服务端正常运行会有类似以下提示


如果出现java运行失败的提示,说明脚本安装java失败,需要手动安装java.


更新
执行一键安装会自动完成更新.

卸载
sh /fs/stop.sh ; rm -rf /fs

启动
sh /fs/start.sh; tail -f /fs/server.log
重复运行启动会出现端口绑定错误,请先停止或直接重启服务.


停止
sh /fs/stop.sh

重新启动
sh /fs/restart.sh; tail -f /fs/server.log

查看日志
tail -f /fs/server.log

设置服务端口
默认udp 150和tcp 150 ,修改端口后服务端会自动修改防火墙.
 mkdir -p /fs/cnf/ ; echo 端口号 > /fs/cnf/listen_port ; sh /fs/restart.sh
注意:由于finalspeed的工作原理,不要开放finalspeed所使用的tcp端口.

设置开机启动
chmod +x /etc/rc.local
vi /etc/rc.local
加入
sh /fs/start.sh

每天晚上3点自动重启
crontab -e
加入
0 3 * * *  sh /fs/restart.sh
