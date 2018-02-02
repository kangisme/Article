## Android Studio 小技巧 ##

### 1. 更改Android Studio工程名 ###
更改包名，类名，变量名等等，一般只需要用到**shift + F6**就可以通通解决，但是有时候需要改projet的名字，选中project然后按shift + F6，然后就会出现下图的情况，让人抓狂。实际很简单，**close project**之后找到项目的存放地址，直接更改项目文件名，然后重新用Android Studio打开项目，等待**Sync**完成即可
![我是图，但炸了](https://i.imgur.com/VekjaOz.png)
### 2. Android Studio调试时源码版本不对应 ###
![我是图，但炸了](https://github.com/kangisme/img/raw/master/%E8%B1%86%E8%8A%BD%E5%9B%BE%E7%89%8720180130135151.png)
有时候调试需要看源代码运行情况，然后调试进去发现如上图这样，一脸懵逼，怎么跑注释上去了。莫慌，这是有原因的，不要忽视，勇于解决问题才是最重要的。这是因为我们使用的测试设备中的系统版本与我们AndroidStudio中使用的SourceCode的版本不一致导致的。比如我们使用的是5.1的设备，而Android Studio错误地识别并使用了版本为7.1的SourceCode，就会出现这个问题。我们从google  Issue Tracker可以知道，该问题是已知bug：[https://issuetracker.google.com/issues/37058409](https://issuetracker.google.com/issues/37058409)

**解决方法**  
**偷梁换柱：将SourceCode人为替换掉**  
我们可以在我们的SDK目录下找到 sources目录，这里面存放的是我们所下载的各个系统版本的源码资源，找到发生错误的源码资源，再用正确的资源替换掉。如何查看Android Studio当前使用的源码版本？在Android Studio打开任一源码类，按Alt + F1选择Show in Explorer，就可以打开sources目录中当前查看的源码。

比如针对我们上面5.1（API 22）系统被错误使用7.1（API 25）的问题，我们进入到sources目录，将android-25目录名称改为android-26-alter，再复制一个android-23，将复制出来的目录改为android-26。之后重启Android Studio进行debug就会发现可以正常对应了，强烈推荐使用该方法。另外如果发现sources中没有我们要找的版本，那就说明我们还没有下载该版本的sourcecode，这需要需SDK中将对应的sourcecode下载一下即可。

**改头换面：将配置文件中的compileSdkVersion和buildToolsVersion都改为指定API**  
该方法需要我们将build.gradle中的配置进行修改，使其改为我们设备对应的版本。该方法适用性有限，因为改完之后常常会出现一堆的编译报错，而且对于大工程来说，这种改编译版本的方法代价实在太大，所以这里并不是十分推荐。
