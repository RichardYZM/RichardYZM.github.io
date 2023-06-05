---
title: 在 Window下调试 ios 的 Safari 浏览器
cover: https://images.pexels.com/photos/147411/italy-mountains-dawn-daybreak-147411.jpeg
date: 2022-11-02 12:00:00
updated: 2022-11-02 12:00:00
---
# 1. 安装 iTunes
Windows 首先要安装 iTunes，下载地址：[iTunes](https://www.apple.com/itunes/)
# 2. iPhone开启调试模式
1. 使用数据线将ios设备与电脑连接
2. 在ios设备上打开 **`设置 > Safari > 高级> 网页检查器 > 启用`**。
# 3. 安装scoop
1. 使用PowerShell在你当前Windows的账户下执行
```powershell
set-executionpolicy remotesigned -s cu
```
2. 下载scoop
```powershell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```
# 4. 安装ios-webkit-debug-proxy
```powershell
scoop bucket add extras
scoop install ios-webkit-debug-proxy
```
如图，点击允许访问
![在这里插入图片描述](../img/%E5%9C%A8%20Window%E4%B8%8B%E8%B0%83%E8%AF%95%20ios%20%E7%9A%84%20Safari%20%E6%B5%8F%E8%A7%88%E5%99%A8/a.webp)
# 5. 开始调试
1. 浏览器输入：`http://localhost:9221/`,这里会显示所有已连接的设备清单，选择一个设备并点击打开
2. 可以发现地址栏变为：http://localhost:9222/, 同时显示该ios设备中Safari浏览器打开的所有页面，和一个提示：
```
Inspectable pages for iPad:
http://********
Note: Your browser may block 1,2 the above links with JavaScript console error:` 
       Not allowed to load local resource: chrome-devtools://...
To open a link: right-click on the link (control-click on Mac), 'Copy Link Address', and paste it into address bar.
```
3. 提示浏览器禁止页面加载本地资源，需在上面的链接（即safari显示已打开页面的链接）点击右键复制链接，然后手动新建一个标签页将链接粘贴进去，回车访问。
```
chrome-devtools://devtools/bundled/inspector.html?ws=localhost:9222/devtools/page/1
```
# 6. 再次调试
执行命令
```powershell
ios_webkit_debug_proxy -f chrome-devtools://devtools/bundled/inspector.html
```
然后重复上面第5步