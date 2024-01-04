# node

## nodejs和npm的关系
1. node.js是javascript的一种运行环境，是对Google V8引擎进行的封装，是一个服务器端的javascript的解释器
2. npm是nodejs的包管理器（package manager）
3. nodejs和npm是包含关系，nodejs中含有npm，安装好nodejs，cmd输入npm -v会发现npm的版本号，说明npm已经安装好

## nvm命令
```Shell
nvm off                      # 禁用node.js版本管理(不卸载任何东西)
nvm on                       # 启用node.js版本管理
nvm install <version>        # 安装node.js的命名 version是版本号 例如：nvm install 8.12.0
nvm uninstall <version>      # 卸载node.js是的命令，卸载指定版本的nodejs，当安装失败时卸载使用
nvm ls                       # 显示所有安装的node.js版本
nvm list available           # 显示可以安装的所有node.js的版本
nvm use <version>            # 切换到使用指定的nodejs版本
nvm v                        # 显示nvm版本
nvm install stable           # 安装最新稳定版
nvm alias default node
```

## node命令
```Shell
# 最新版本
npm install npm@latest -g
# 指定版本
npm install npm@××.×× -g

npm config set registry https://registry.npm.taobao.org
npm config get registry

```