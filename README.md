# NUAA-Check i南航全自动打卡系统
## （第一次使用极其的麻烦是为了后面的方便）
## 更新日志

+ 2022.5.10：已确认可正常使用。
+ 2022.5.16：key字符串长度大于32会自动截断，微信推送标题直接显示“打卡成功”或“打卡失败”，不用每次都点开

## 使用方法
1. 将本项目fork到自己的仓库（对，就是有上有一个fork按钮）
2. 在**自己的**github的settings(对，就是上面一栏code issues最后一个，点进去左边找到secrets里面设置下secrets,详见[参数配置](#canshu)
3. clone自己的项目一份到本地，使用`pip install -r requirements.txt`安装项目依赖,新建文件key.txt，填写内容为上步中设置的key
4. 修改.github/workflows/nuaa.yml第19行`git clone https://github.com/li1553770945/NUAA-Check .`，把li1553770945换成你的github用户名
5. 使用谷歌或edge浏览器打开 [打卡链接](https://m.nuaa.edu.cn/ncov/wap/default/index) ，按F12打开控制台，手动进行打卡，按照如下方式复制curl命令

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509142654.png)

6. 在项目目录新建data.txt，把刚刚复制的内容粘贴进去，**运行encrypt.py**（不要遗漏！），这是应该会看到encrypt.txt的内容发生了变化，这是你的加密之后的打卡请求 
7. 提交并push新的代码
8. 到自己仓库的Actions查看，一般默认workflow是关闭，务必打开workflow，如下状态是正常的

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509234354.png)

如果是刚刚Enable，需要再次push一次，你可以随便修改一点README的内容然后再次push，就会触发action。点进action应该可以看到有一个绿色的对号，如果是黄色的则为正在运行，红色的为运行失败，如果没有任何内容为还没有开启，请在确认Enable了之后再次push。

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509234614.png)

打卡结果会通过微信推送到你的手机上。

<h2 id="canshu">参数配置</h2>

sckey：在 [Server酱](https://sct.ftqq.com/sendkey) 绑定微信找到SendKey填入，这是为了可以收到打卡结果的反馈。

复制下图所示sendkey。

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509145808.png)

新建如下sckey:

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509144008.png)

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509145951.png)

再新建一个，名字为 key,是加解密的密钥，自己定义的一串**长度为32**的不规则字符（下面一个步骤还会用到），例如:"lo9878iij6vfdni09fp9l0p12fgy6los"（请不要把密钥告知他人，也不要使用这个实例作为密钥）

![](https://cdn.jsdelivr.net/gh/li1553770945/images/20220509150125.png)

## 注意：
1. github的workflow过一段时间会失效（github会发邮件通知）,需要手动重新开启
2. 如果您要修改打卡时间，请修改nuaa.yaml中的` - cron: '01 16 * * *'`,01代表分钟，16代表小时。请注时间是GMT时间，也就是比北京时间慢8个小时。
3. 程序设定每天00:01打卡，但是经过实际测试有很长延迟，可能为20分钟到一小时
4. 如果后续打卡内容出现变化或者登陆失效，您只要更重复使用方法——步骤5-7即可

## 常见问题

### 1. 为什么不设计称一个POST请求多方便？
这样设计是很方便，但是如果打卡内容发生变化，必须手动更新代码。这样设计第一次使用的时候麻烦，但是可以方便应对以后提交内容发生变化的情况。

### 2. 为什么一段时间后开始提示打卡失败了？

可能是登陆失效或提交内容发生变化，只要重复步骤5-7。


### 免责声明
本程序仅供学习参考，不得利用本程序替代手动打卡使用，如出现瞒报等行为与本程序无关。请在clone或fork后24小时内删除，否则后果自负！


