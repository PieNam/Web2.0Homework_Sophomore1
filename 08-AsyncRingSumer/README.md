# Asynchronous Ring Sumer

## 检查方式

1.打开终端/命令行

2.cd进入项目文件夹目录/AsyncRingSumer

​	cd yourOwnDir/AsyncRingSumer

3.启动Server Side：

​	node server.js

4.收到服务器启动信息：Asynchronous Ring Sumer server is listening at 3000

​	打开浏览器，地址栏中输入：

​		http://localhost:3000/S1/index.html

​		即可开始查看任务S1运行情况，**S2-5以此类推。**



## 任务实现及说明

在Homework 3 - Ring Menu代码的基础上，添加与服务器交互的Ajax代码，环形菜单变身为环形Async求和计算器。

**下面是每个任务相应的实现说明。**



### S1：人工交互

完成从环形菜单，向环形Async求和计算器的转变。计算器交互有以下约束：

1. A~E的按钮：
	1. 在未得到随机数之前，不显示右上角的红色圆圈
	2. 用户点击某个按钮，将显示其对应红色圆圈，并发送请求到服务器获取随机数
	3. 在得到结果前
		1. 红色圆圈内显示 。。。，表示正在等待随机数
		2. 灭活（disable）其它按钮，变为灰色，用户不能够点击（点击没有响应，也不会发送新的请求到服务器）
	4. 得到服务器发回的随机数后，
		1. 显示在当前按钮右上角红色圆圈内
		2. 灭活当前按钮，变为灰色，用户不能够点击
		3. 激活（enable）其余按钮，呈现为蓝色，用户可以点击，从服务器获取随机数
2. 大气泡
	1. 在A~E未能全部得到自己的随机数之前
		1. 大气泡内无任何显示
		2. 灭活大气泡，用户不能够点击
	2. 当A~E按钮全部获得了自己的随机数时，激活大气泡
	3. 此时，点击大气泡
		1. 计算A~E随机数的和，显示在大气泡内
		2. 灭活大气泡
3. @+按钮
	1. 任何时候，鼠标离开@+区域，将重置整个计算器，清除所有A~E按钮的随机数和大气泡内的和
	2. 鼠标再次指向@+，可以开始新一轮的计算操作

#### 说明

​	**S1任务中，在A～E按钮均得到随机数后，点击大气泡会进行求和计算与显示，同时五个小按钮会恢复活性，此时点击任意A～E按钮，得到回传数据之后，由于五个按钮仍然都具有各自的随机数，大气泡会再次激活，可以再次点击大气泡求和。（由于原先Ring Menu版式设计，大气泡的有效点击范围不是一个完整的圆，与界面环重叠部分点击无效，但由于与本次重点无关，便不加改进了。）**

​	**除了此处功能调整之外，其余功能和逻辑按照题目要求实现。若有纰漏，还请同学指正！**



### S2：仿真机器人，顺序（一指禅）

在满足S1的各种约束的情况下，设计一段机器人程序，点击@+按钮，将执行这段机器人程序：

1. 模拟自动执行从A~E点击按钮，获取随机数，然后点击大气泡求和的过程
2. 要求完全模拟全过程，包括红圈的出现、随机数的显示、和的显示，以及按钮的激活与灭活
3. 重点：必须严格的模拟按A~E、然后大气泡的顺序点击按钮的过程
	1. 例如：当机器人按下A之后，将等待A的随机数显示，其余B~E按钮激活
	2. 此时，机器人将立即按下B
	3. 此后，与此相似，以此类推……

#### 说明

​	**为了展示显示效果，S2、S4、S5机器人都添加了操作延迟，以模拟点击效果。**

​	**因为机器人系列任务中，是要求实现机器人自动点击按钮，所以在这些任务中，设置了无法人工点击按钮以避免扰乱机器人逻辑而造成歧义。同时，每次点击机器人执行完任务显示加和之后，流程终止，若想再次启动机器人需离开环形菜单再次进入，或者刷新页面以重置。**



### S3：仿真机器人，并行（五指金龙）

同S2一样，设计一段机器人程序，点击@+按钮，将模拟同时按下A~E按钮，并在所有随机数到达后，立即按下大气泡的过程。

1. 根据S1的规则，初始时，A~E 5个按钮都是可以按下的
2. 这时，机器人5指齐按，同时向server发出请求
3. 机器人观察到所有数据到达后，再按下大气泡

#### 说明

​	**此任务需要五个请求同时处理并返回值，所以此处向服务器取数改为ajax，其他四个任务因为影响不大，保留get方法。**

​	**每次点击机器人执行完任务显示加和之后，流程终止，若想再次启动机器人需重新打开页面或离开环形菜单再次进入以重置（若反复快速地离开菜单再次点击，会导致服务器重复发送数据，这是一个小BUG）。同时，由于要求中未规定数据回传顺序，在此程序中默认以A-B-C-D-E顺序收到随机数。**



### S4：仿真机器人，随机（乱点鸳鸯）

同S2一样，设计一段机器人程序，点击@+按钮，将模拟随机单击A~E按钮，并在所有随机数到达后，立即按下大气泡的过程。

1. 点击@+后，机器人先计算一个点击A~E的随机顺序，例如：（B、C、E、A、D），并将此顺序显示在大气泡上方
2. 机器人仿真模拟按此顺序点击按钮求和的情况

#### 说明

​	**机器人除了动作逻辑依照S4要求实现之外，其他与S2、S3相同。**



### S5：仿真机器人，独立行为（终极秘密） 

类似S4，先计算并显示一个即将操作的顺序，然后机器人进行点击按钮。不同的是，要求A、B、C、D、E和大气泡不能够共享事件处理代码。

1. 每个小按钮、大气泡独立的写一个处理函数（handler），例如：aHandler、bHandler、cHandler、dHandler、eHandler、bubbleHandler
2. 计算出操作顺序后，机器人必须按顺序依次调用各handler，传递参数和处理异常
	1. 各handler逻辑：除了与S4行为相同，取服务器随机数，最后计算之外，还要实现下述行为：
		1. 在大气泡上方显示一句话（message），分别为：
			1. A：这是个天大的秘密
			2. B：我不知道
			3. C：你不知道
			4. D：他不知道
			5. E：才怪
			6. 大气泡：楼主异步调用战斗力感人，目测不超过xxx，xxx为a~e随机数的和
	2. 分布求和与参数传递：求和过程分步完成，各handler从参数接收截止到目前的累积和（currentSum），并和自己取回的随机数相加以后，在调用下一个handler时，作为参数传递过去
	3. 异常处理：
		1. 各handler的处理会随机失败，失败的handler不输出message，而是传回{message，currentSum}
			1. message为原定message的否定形式
			2. currentSum即为当前计算出的累计和
		2. 调用者必须处理异常，保证整个应用程序正常，执行过程不变。
			1. 调用者根据异常，帮callee显示message
			2. 调用者执行自己的逻辑
	4. 不允许使用任何全局变量！

#### 说明

​	**已有注意到对全局变量的限定，代码中所用变量仅为展示效果：设置handler处理失败的概率、生成并打印随机序列、统一机器人操作延迟时间。并未利用全局变量实现异步方法嵌套。若这样的写法不符合要求还麻烦指出改进！**

​	**离开环形菜单待其完全收缩之后，可以再次唤醒环形菜单，点击机器人，执行逻辑。**

​	**对应message否定形式：**

​		A：这是个天大的秘密 - 这不是个天大的秘密

​		B：我不知道 - 我知道

​		C：你不知道 - 你知道

​		D：他不知道 - 他知道

​		E：才怪 - 不才怪（才不怪？）

​		大气泡：楼主异步调用战斗力感人，目测不超过xxx

​				楼主异步调用战斗力好菜，目测超过xxx

​	**各Message显示后新Message到来之后会被覆盖。**

​	**对附加项目理解浅薄，如果有不到位的地方还请指正！一定加以考虑修改！**



