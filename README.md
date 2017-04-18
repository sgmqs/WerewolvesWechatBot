# 狼人杀法官.BOT

这个程序是基于 itchat，用 Python3 写成的狼人杀面杀助手，用于在微信上作为机器人法官。

目前此程序在实战中使用了4次，但是不是系统性地测试，可能依然不太稳定。

## 系统要求

理论上只要成功安装 pyaudio 和 itchat 即可运行，而这两个依赖项均同时支持 Linux, Mac 和 Windows。不过，我只在 Linux 上测试了程序。

## 安装方法

程序需要以下依赖项:

* pyaudio
* itchat

可以使用 pip 安装：

	pip3 install pyaudio
	pip3 install itchat

## 使用方法

安装完成后，使用 python3 运行主程序：

	python3 werewolf_server.py

运行后，itchat 会弹出微信登录二维码，用微信扫描后确认登录即可成为法官。
	
**提示：**

1. 如果想一个人试用这个程序（用文件传输助手），可以让`werewolf_server.py` 中的 `GameController.is_game_ended` 方法直接 `return` 以阻止游戏结束（没有返回值）。否则，由于只有一位玩家，程序会在第一晚认为刀边成功。
2. 如果登录为法官的用户需要使用自己的帐号参加游戏（用文件传输助手），可以在游戏时把微信设置成勿扰模式，以免不小心从消息提示里看到别人的身份。

### 命令列表

用微信向法官发送以下内容即可执行相应的指令（如果法官是自己，可以用文件传输助手发送）：

* 进入游戏：进入游戏并初始化自己的身份。法官会提示如何进行初始化。**注意：**对于没有发送过“进入游戏”的用户（包括文件传输助手），法官不会响应其他的命令。
* 编辑配置：交互式地编辑配置文件，可以设置各种身份的数量、游戏规则等。游戏进行过程中不可用。
* 查看配置：查看当前身份配置和自己的身份。
* 开始游戏：检查玩家是否均已进入游戏，如果到齐则开始游戏。**注意：**未完成初始化的玩家也会被认为还没进入游戏。
* 接管上帝：查看所有玩家的身份和游戏历史。**注意：**任何玩家都可以接管上帝，包括未死亡的玩家，但是接管上帝的操作会告知其他玩家。

## 功能

* 分配身份并主持夜晚的行动
* 电脑语音提示（由于微信的接口限制，暂不能用微信语音来播放提示）
* 实现的角色：
	* 神阵营
		* 预言家
		* 女巫
			* 可以设置能否双开
			* 可以设置第一晚能否自救
			* 可以设置第二晚之后能否自救
			* 同守同救为死
		* 守卫
			* 可以设置能否连续守同一个人
		* 猎人（被毒死不能开枪）
		* 白痴
	* 狼阵营
		* 白狼王
		* 狼人
	* 村民
* 主持警长竞选
* 随机选择发言顺序
* 投票系统、票形统计

**注意：**目前游戏是刀边规则，暂未添加屠城规则。

## 文件结构

* audio（文件夹）：包含语音提示的声音文件
* werewolf_server.py：主程序，包含游戏控制器的代码
* charactor.py：包含所有游戏角色的代码
* wechat.py：包含发送、接受微信消息，响应命令的代码
* audio.py：包含播放声音的代码
* config_editor.py：包含控制配置文件的代码
* config.json：游戏的配置文件
* config_prompts.json：储存配置文件中每个值对应的菜单提示

## 技术问题笔记

此处用于记录开发过程中遇到过的问题：

1. 目前 audio.py 只能读取 signed 16-bit PCM 格式的 WAV 文件，读取 32-bit float PCM 格式的 WAV 会崩溃。
2. 如果程序发送消息的频率过快，则微信帐号会被屏蔽一段时间。之前出现了单人测试没有问题但是多人测试的时候微信号被屏蔽的情况，所以减少了群发消息的使用。