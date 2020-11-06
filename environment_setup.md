

# 环境搭建 

## 通过USB连接手机 

1. 安装`usbmuxd `库，该库带有`iproxy`服务

执行`iproxy 2222 22` 将本地2222端口映射到设备22端口 

2. 生成本地密钥，导入到远程信任列表，避免每次连接都要密码验证

ssh-keygen -t rsa -P ''

ssh-copy-id -i ~/.ssh/id_rsa root@localhost

初始密码：[`alpine`]

3. 连接

ssh root@localhost -p 2222


## mac安装frida-ios-dump 

1. 手机安装frida

1.1 手机越狱之后在 Cydia --> 软件源 --> 编辑 --> 添加源(build.frida.re)
1.2 进入 build.frida.re 源下载 Frida

2. mac下，使用系统py版本

2.1 sudo pip install frida

2.2 下载 https://github.com/AloneMonkey/frida-ios-dump.git

2.3 

sudo pip install frida --upgrade --ignore-installed six

sudo pip install -r requirements.txt --user

2.4 开始砸壳

开启 iproxy

终端连接手机 

另一终端，cd `frida-ios-dump`， 

运行 `dump.py -l`

运行 `dump.py 全军出击`

3. **出现报错:**

3.1 No handlers could be found for logger "paramiko.transport"

    解决：pip install paramiko --upgrade 更新paramiko

3.2 [Errno 60] Operation timed out

    解决： 未解决

## mac 安装theos

越狱开发工具包

Mac OS 10.13版本之后禁止了软件以root身份在Mac上运行

解决办法：关闭SIP
1.重启Mac，按住Command + R键直到Apple Logo出现，进入Recovery Mode模式
2.点击工具里的Terminal（终端）
3.执行 csrutil disable
4.重启Mac
5.重启完成后，执行 sudo chflags norestricted /usr/local && sudo chown -R jc /usr/local
（如果想重新开启安全设置，则重复1、2步骤，输入csrutil enable就可以了）

1. dpkg 

Debian Packager, 我们可以使用dpkg来制作deb，Theos开发的插件都将会以deb的格式进行发布的

brew install dpkg

2. ldid 

在Theos开发插件中，iOS文件的签名是使用ldid工具来完成的，也就是说ldid取代了Xcode自带的Codesign。

brew install ldid

3. theos

cd /usr/local/opt/

git clone --recursive https://github.com/theos/theos.git

sudo chown -R $jc:$staff theos

4. 使用theos创建，编译，安装一个自己的工具

export THEOS=/usr/local/opt/theos
export SDKVERSION=10.3
export THEOS_DEVICE_IP=localhost
export THEOS_DEVICE_PORT=2222 

xcodebuild -showsdks 查看sdk version

4.1 新建工程
$THEOS/bin/nic.pl

4.2 make, make package, make install 

4.3 在cydia deb列表即可看到工具


## mac安装monkeydev

