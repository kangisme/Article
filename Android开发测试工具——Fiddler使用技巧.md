## Android开发测试工具——Fiddler使用技巧 ##

### 1.Fiddler基本设置 ###
**电脑端设置**<br/>
安装完成后，首先点击tools->options->Connections,如下图所示，设置Fiddler监听端口（默认为8888，我设置的是6666，可设置其它）以及允许远程电脑连接
![](https://i.imgur.com/WT7z0LL.png)
然后，打开Windows的命令行（快捷操作提示：按住shift右键，选择**在此打开命令行窗口**），输入ipconfig，获取当前电脑IP地址

**Android手机设置**<br/>
手机需要和电脑在同一个局域网内，或者笔记本可以用360免费WiFi建立一个局域网，然后手机连上。打开手机**设置->WLAN**选择要连接的WiFi，长按后点击修改网络，点击**显示高级选项**，如下图所示，**代理服务器主机名**填查询到的IP地址，**代理服务器端口**填Fiddler的监听端口<br/>![](https://raw.githubusercontent.com/kangisme/-/master/Screenshot_2017-12-27-15-27-20.png)

### 2.抓包 ###
基本设置完成后，开始抓包，在打开Fiddler的情况下，手机使用Http协议访问网络的数据包都会被Fiddler抓获。如下图所示，左侧是抓取的数据包，右侧是数据包中的json数据。
![](https://github.com/kangisme/-/raw/master/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20171227175258.png)
### 3.更改某个请求的响应 ###
选中某个请求后，可在右边**Inspectors**选项中查看其详细信息，包括Http协议头、WebForms、整个Http协议（**Raw**）等信息。通过这些信息，可以完全弄清楚整个http请求到底做了什么。在**AutoResponder**选项中，可以更改这个请求的响应，你想让它给你什么，它就会给你什么。具体操作如下图
![](https://github.com/kangisme/-/raw/master/Fiddler%E6%9B%B4%E6%94%B9response.png)
### 4.模拟低网速环境 ###
在日常的网络测试中，经常需要测试网络超时或在网络传输速率不佳的情况的应用场景，而与此同时我们有时手边资源有限，实现在各种真实网络（2G\3G）环境下测试有些局限性。其实 fiddler 已经提供了类似的功能，通过限定数据的传输速率，近似模仿各种网络场景，虽不精确，但确实一种非常不错的网络环境模拟手段！

那么，fiddler 具体是如何实现的呢？
启用模拟低网速环境路径**Rules/Performances/Simulate Modem Speeds**，启用后你会明显感觉到网速相对之前变慢了许多，尤其是在资源文件比较大的时候。
如果还没有达到低速的要求，那就手动限速<br/>
设置限速路径**Rules->Customize Rules...**或者快捷键**【Ctrl + R】**直接打开fiddler规则脚本页面，**【Ctrl + F】**查找到如下图红框所示内容，时间单位为毫秒，是每上传/下载1KB所需耗时。可以根据需要来进行测试，知道满足开发和测试需求。需要注意的是，保存规则脚本后，**Rules/Performances/Simulate Modem Speeds**会默认取消勾选，得重新勾选才能使脚本生效。
![](https://github.com/kangisme/-/raw/master/fiddler%E8%A7%84%E5%88%99%E8%84%9A%E6%9C%AC.png)

更多关于Fiddler的知识可参考：  
[【HTTP】Fiddler（一） - Fiddler简介](http://blog.csdn.net/ohmygirl/article/details/17846199)  
[【HTTP】Fiddler（二） - 使用Fiddler做抓包分析](http://blog.csdn.net/ohmygirl/article/details/17849983)  
[【HTTP】Fiddler（三）- Fiddler命令行和HTTP断点调试](http://blog.csdn.net/ohmygirl/article/details/17855031)