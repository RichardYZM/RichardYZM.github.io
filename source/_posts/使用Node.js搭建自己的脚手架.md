---
title: 使用Node.js搭建自己的脚手架
cover: https://images.pexels.com/photos/3022417/pexels-photo-3022417.jpeg
date: 2022-06-12 12:00:00
updated: 2022-06-12 12:00:00
---
## 1. 用到的工具
####  Commander 命令行工具
##### 1. version 定义脚手架的版本
##### 2. command 添加命令行名称
 1. 命令名称：后面可跟 <> 或 [ ] 包裹的参数，传入的参数会被传入到 action 回调函数中
 2. 命令描述：如果存在，且没有显示调用 action, 就会启动子命令程序，否则会报错
 3. 配置选项 <可省略>： 可配置 noHelp, isDefault 等
##### 3. description 定义命令的描述
##### 4. action 定义命令的回调函数
```javascript
program
    .version(_v, '-v, --version')
    .command('list')
    .description('查看所有可用的项目模板')
    .action(() => {
        console.log(`
        ${chalk.green('1. ReactAdmin')}
        ${chalk.red('2. ReactWeb')}
        ${chalk.blue('3. ReactH5')}
        `
        )
    })
```

####  download-git-repo 模板下载工具
 1. 这是用来下载远程模板的，支持 GitHub、 GitLab 和 Bitbucket 等
```javascript
let download = require('download-git-repo'); // 模板仓库下载工具
download(repository, destination, callback)
// repository 是远程仓库地址
// destination 是存放下载的文件路径，也可以直接写文件名，默认就是当前目录
// callback 下载之后的回调函数，接收一个参数
```
####  inquirer 命令行交互工具
 2. 这是个强大的交互式命令行工具,如果你想要和用户进行交互,那么你需要使用它,具体用法如下:
```javascript
                //命令行交互
                inquirer.prompt([
                    {
                        type: 'input',
                        name: 'name',
                        message: '请输入项目名称',
                        default: projectName
                    },
                    {
                        type: 'input',
                        name: 'description',
                        message: '请输入项目简介',
                        default: ''
                    },
                    {
                        type: 'input',
                        name: 'author',
                        message: '请输入作者名称',
                        default: ''
                    }
                ]).then((answers) => {
                    //根据命令行答询结果修改package.json文件
                    fs.readFile(`${projectName}/package.json`, 'utf8', function (err, data) {
                        if (err) {
                            console.log(chalk.red('读取配置失败'));
                            return;
                        }
                        let package = JSON.parse(data)
                        Object.assign(package, answers);
                        package = JSON.stringify(package, null, 4);
                        fs.writeFile(`${projectName}/package.json`, package, 'utf8', (err) => {
                            if (err) {
                                console.log(chalk.red('修改配置失败'));
                                return;
                            }
                            console.log(chalk.green('项目初始化成功'))
                        });
                    })
                })
```
####  ora 命令行加载进度标识
 3. 这是一个好看的加载，就是你下载的时候会有个转圈圈的那种效果，用法如下：
```javascript
const ora = require('ora')
let spinner = ora('downloading template ...')
// spinner.start()
// spinner.fail('项目模板下载失败');
// spinner.succeed('项目模板下载成功');
...
```
####  chalk 命令行输出字符样式
 4. 这是用来修改控制台输出内容样式的，比如颜色,加粗,字体背景色等等，具体用法如下:
```javascript
const chalk = require('chalk');
console.log(chalk.green('success'));
...
```
## 2. 脚手架项目搭建
1. 新建文件夹，npm init创建项目package.json文件
2. 修改package.json文件的bin字段，设置脚手架的入口文件
3. 新建入口文件index.js,在入口文件顶部添加代码#! /usr/bin/env node,声明该命令行脚本是node.js写的
4. 下载所需要的工具依赖包
## 3. 设计指令
```javascript
// my-cli -h                 //查看脚手架的配置项和具有的功能
// my-cli -v                 //查看脚手架版本号
// my-cli list               //查看脚手架可用的模板列表    
// my-cli init 模板名 项目名  //脚手架初始化模板项目
```
## 4. 代码编写
```javascript
#! /usr/bin/env node

let program = require('commander')
let fs = require('fs')
let chalk = require('chalk');
let ora = require('ora')
let download = require('download-git-repo'); // 模板仓库下载工具
let inquirer = require('inquirer')
// 取得包版本号
let _v = require('../package.json').version;


// my-cli -h                 //查看脚手架的配置项和具有的功能
// my-cli -v                 //查看脚手架版本号
// my-cli list               //查看脚手架可用的模板列表    
// my-cli init 模板名 项目名  //脚手架初始化模板项目


let templates = {
    'ReactAdmin': {
        url: 'https://github.com/RichardYZM/Templates-Cli',
        downloadUrl: 'RichardYZM/Templates-Cli#main',
        description: 'React模板'
    },
    'ReactWeb': {
        url: 'https://github.com/RichardYZM/Templates-Cli',
        downloadUrl: 'RichardYZM/Templates-Cli#ReactWeb',
        description: 'React模板'
    },
    'ReactH5': {
        url: 'https://github.com/RichardYZM/Templates-Cli',
        downloadUrl: 'RichardYZM/Templates-Cli#ReactH5',
        description: 'React模板'
    },
}

program
    .version(_v, '-v, --version')
    .command('list')
    .description('查看所有可用的项目模板')
    .action(() => {
        console.log(`
        ${chalk.green('1. ReactAdmin')}
        ${chalk.red('2. ReactWeb')}
        ${chalk.blue('3. ReactH5')}
        `
        )
    })

program
    .command('init <template> <project>')
    .description('项目初始化')
    .action((templateName, projectName) => {
        const spinner = ora('正在下载模板...').start();
        if (!(templateName in templates)) {
            spinner.fail(chalk.red('下载模板出错,请输入正确的模板'));
            return;
        }
        let { downloadUrl } = templates[templateName];
        //github，项目名
        download(downloadUrl, projectName, err => {
            if (err) {
                spinner.fail(chalk.red('下载模板出错'));
            } else {
                spinner.succeed(chalk.green('下载模板成功'));

                //命令行交互
                inquirer.prompt([
                    {
                        type: 'input',
                        name: 'name',
                        message: '请输入项目名称',
                        default: projectName
                    },
                    {
                        type: 'input',
                        name: 'description',
                        message: '请输入项目简介',
                        default: ''
                    },
                    {
                        type: 'input',
                        name: 'author',
                        message: '请输入作者名称',
                        default: ''
                    }
                ]).then((answers) => {
                    //根据命令行答询结果修改package.json文件
                    fs.readFile(`${projectName}/package.json`, 'utf8', function (err, data) {
                        if (err) {
                            console.log(chalk.red('读取配置失败'));
                            return;
                        }
                        let package = JSON.parse(data)
                        Object.assign(package, answers);
                        package = JSON.stringify(package, null, 4);
                        fs.writeFile(`${projectName}/package.json`, package, 'utf8', (err) => {
                            if (err) {
                                console.log(chalk.red('修改配置失败'));
                                return;
                            }
                            console.log(chalk.green('项目初始化成功'))
                        });
                    })
                })
            }
        })
    })




program.parse(process.argv);  // 解析命令行参数
```
## 5. 上传至npm
1. 官网注册账号：https://www.npmjs.com/ 注册后需查看邮箱信息，激活验证才可上传
2. 切换npm地址：npm config set registry https://registry.npmjs.org （在安装了淘宝镜像的情况下）
3. 登录：npm login（一般第五步，会提示输入账号）
4. 发布包，上传到npm服务器：npm publish
5. 删除npm包，后面是版本号：npm unpublish richard-cli@1.0.0