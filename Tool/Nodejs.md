# Node.js

- 安装
```
pacman -S nodejs npm python2
```

- 调试
```
npm install -g node-inspector
node-debug app.js
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
npm install githubname/reponame 从Github上的项目源代码安装package
npm update <packageName>     更新已安装模块
npm rebuild node-sass        重新编译node-sass
npm outdated                 检测当前安装的所有模块是否有更新
npm dedupe                   重新计算依赖关系，优化模块的存放结构
npm run                      列出在package.json文件中定义的脚本命令
```
- 卸载模块
```
npm uninstall <packageName> -S 卸载并更新package.json文件
```

- 使用国内的镜像服务
```
npm config ls
npm config set registry https://registry.npm.taobao.org
```

- [Yarn](https://yarnpkg.com/)
```
packer -S yarn
yarn init
yarn info <package>
yarn add <package>
yarn add <package> --dev
yarn add <package> --peer
yarn add <package>@<version>
yarn add <package>@<tag>
yarn upgrade <package>
yarn remove <package>
yarn why <query>
yarn outdated
yarn upgrade
yarn ls
yarn
yarn start
yarn test
yarn run <script>
```
