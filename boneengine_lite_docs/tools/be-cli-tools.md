
# 准备工作

下述示例中的日志信息是基于你已烧录了 TinyEngine固件前期条件，并将使用固件自带的TinyEngine App程序做为演示示例。

目前仅支持串口调试，下一个版本支持网络调试设备！！！

* esp32模组设备刷写TinyEngine固件
* 安装node.js 链接， node >= 6.4.0
* 安装be-cli工具，支持串口和网络更新app.bin(js程序包)
```bash
npm i be-cli -g -P
be -V
```

或者根据TinyEngine源码，在LiteFramework 目录下，通过 make 安装:
```plain
make cli
```

如果执行上述全局安装之后 运行be还提示找不到命令，则执行下面命令得到Nodejs的模块安装路径

npm prefix -g

将输出的全局安装路径加入的环境变量的Path条目中即可
比如我的机器：
__vi ~__/.bash\_profile
添加下面一行
export PATH=$PATH:/usr/local/lib/node\_modules/node/bin
使之生效
source ~/.bash\_profile

Q/A：有时由于be-cli 前后版本兼容性问题，导致be-cli升级失败，这时需要手工删除 node 下的 node-modules/be-cli 整个目录后再安装，后续版本会以更优雅的方式安装be-cli工具

# 使用
```javascript
Usage: be [options]
develop js app on aos
Options:

-V, --version                   output the version number
-i, --init                      init project by appName and chip:--init demo esp32
-p, --pack                      pack app dir
-u, --upload                    upload bin file to device
--verbose    <verbose>          open/close verbose(1/0) (default: 1)
--baudrate   <baudrate>         baudrate default: 115200 (default: 115200)
--databits   <databits>         databits default: 8 (default: 8)
--parity     <parity>           parity default: none (default: none)
--stopbits   <bits>             stop bits default: 1 (default: 1)
devices                         list out all devices include network device and uart device
scan                            scan devices include serial and local network
start                           start the application on selected device
stop                            stop the application on selected device
restart                         restart the application on selected device
reboot                          reboot the device
dumpsys                         dumpsys info:dumpsys task:dumpsys mm_info
pull                            pull file
list                           `list folderPath` print out files under folderPath
install                        `install app-package-name devPath` to devPath
uninstall                      `uninstall index.js/folderPath` from device, `am uninstall format` will format spiffs
push                           `push index.js?/app.bin? dir` push file to device path dir
bshell                          BoneEngine shell
sysshell                        Device Uart Console
ifconfig                        get network config information
connect                         connect to input device
disconnect                      disconnect from the connected device
rename                          rename device
dmesg                           print out all message
logcat                         `logcat string` to grep string from log message
wifi                            send wifi ssid and passowrd to device to connect
-h, --help                      output usage information
```


## 1.项目初始化
  执行```be -i demo_app developerkit```

执行该命令会在当前目录下创建生成一个 "demo\_app" 的文件夹


be -i appName  chipName
其中chipName=esp32devkitc/developerkit

如果用户默认两个参数都不传递，则默认创建 gravity\_demo的文件夹，使用esp32devkitc的默认配置


如果用户只传递第一个参数，则默认第一个参数为appName，使用默认esp32devkitc配置生成项目。

## 2.生成应用程序包：app.bin
 执行```be -p demo_app```
 该命令会将demo_app目录打包一个 app.bin 文件

##3.查看当前可用TinyEngine目标设备

包括串口设备和当前网络下的设备
 ```be devices```

##4.连接指定的TinyEngine目标设备

目前仅支持连接串口设备，网络设备在下一阶段支持。
```be connnect```


此时如果再次执行be devices会看到所连接的设备高亮了


## 5.连接wifi热点
  ```be wifi wifi_ssid wifi_password```
 将wifi_ssid和wifi_password替换成你需要连接到wifi热点。


## 6.push文件【.js/.bin】到TinyEngine目标设备
执行``` be push app.bin /```

该命令会将demo_app目录下的文件push到设备根目录。

设备启动后会自动加载并运行名为index.js的文件。 