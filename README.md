# webpack-demo

运行  node_modules/bin/webpack--node_mudules/webpack

#### webpack.js

判断是否安装了webpack-cli || webpack-command

```
//判断是否安装了依赖包
const isInstalled = packageName => {
	try {
		require.resolve(packageName); //require.resolve 还会在拼接好路径之后检查该路径是否存在, 如果 resolve 的目标路径不存在, 就会抛出 Cannot find module './some-file.txt' 的异常
		return true;
	} catch (err) {
		return false;
	}
};
```

未安装则引导安装, 安装采用npm | yarn 
```
const isYarn = fs.existsSync(path.resolve(process.cwd(), "yarn.lock"));
//安装命令
const runCommand = (command, args) => {
	const cp = require("child_process");
	return new Promise((resolve, reject) => {
		const executedCommand = cp.spawn(command, args, {
			stdio: "inherit",
			shell: true
		});

		executedCommand.on("error", error => {
			reject(error);
		});

		executedCommand.on("exit", code => {
			if (code === 0) {
				resolve();
			} else {
				reject();
			}
		});
	});
};
```

如果安装了某一个webpack-cli || webpack-command, 则引入webpack-cli || webpack-command的入口文件， webpack-cli入口文件是一个自执行函数
```
else if (installedClis.length === 1) {
	const path = require("path");
	const pkgPath = require.resolve(`${installedClis[0].package}/package.json`);
	// eslint-disable-next-line node/no-missing-require
	const pkg = require(pkgPath);
	// eslint-disable-next-line node/no-missing-require
	require(path.resolve(
		path.dirname(pkgPath),
		pkg.bin[installedClis[0].binName]
	));
} 
```


#### webpack-cli.js

process.argv 返回一个数组，其中包含当 Node.js 进程被启动时传入的命令行参数。
yargs是nodejs环境下的命令行参数解析工具,
process.stdin.resume()

