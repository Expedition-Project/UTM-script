# UTM-Script
在Mac OS上实现开机自启动UTM中的虚拟机
本脚本创建在Mac OS sequoia 15.2，UTM 4.6.3，低于此版本不清楚是否可以运行
***
## 使用方法

### 开始设置
在UTM设置里开启关闭窗口继续运行UTM
### 1.创建app
1. 在启动台中找到自动操作app（找不到的在桌面comd+空格搜索）
2. 选择新建文稿-应用程序
3. 在左上角搜索Applescript
4. 打开后删除里面内容将下面代码粘贴进去
```
tell application "UTM"
	do shell script "open 'utm://start?name=你的虚拟机名字'"
	activate -- 启动 UTM 主应用
	delay 1 -- 等待 UTM 加载
	try
		set vm to virtual machine named "你的虚拟机名字"
		start vm
	on error errMsg
		display dialog "启动虚拟机失败: " & errMsg
	end try
end tell

-- 等待几秒后关闭 UTM 窗口（不退出）
delay 3

tell application "System Events"
	tell process "UTM"
		try
			-- 发送 Command + W 关闭窗口
			keystroke "w" using {command down}
		on error errMsg
			display dialog "关闭窗口失败: " & errMsg
		end try
	end tell
end tell
```
5. 将你的虚拟机名字字段修改成UTM中你命名的虚拟机名字
   确保虚拟机名称完全匹配，不要有空格
### 2.运行app开启权限
在左上角文件选择导出命名随意，导出后运行一下，会有两个弹窗请求权限
如果报错请（comd+Q）完全退出UTM，再次运行

确认在
1. 系统设置
2. 隐私与安全性里
3. 辅助动能，自动化
保证app权限都是打开的
### 3.添加登陆项
最后在登陆项添加你命名的程序，这样就可以顺利的开机自启虚拟机
