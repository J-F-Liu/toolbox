# Node.js

- 安装
```
pacman -S nodejs npm python2
```

- 升级npm
```
npm -v
npm install -g npm@latest
```

- 安装模块
```
npm install <packageName>    安装package
npm install <packageName> -S 安装并保存到package.json文件
npm install <packageName> -D 安装并保存到package.json文件的devDependencies部分
npm install <packageName> -f 强制重新安装
npm update <packageName>     更新已安装模块
npm outdated                 检测当前安装的所有模块是否有更新
npm dedupe                   重新计算依赖关系，优化模块的存放结构
```

- 使用国内的镜像服务
```
npm config set strict-ssl false
npm config set registry http://registry.cnpmjs.org
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install [name]
```
