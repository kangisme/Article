在使用AndroidStudio做项目久了，会遇到同一个project下有多个module的情况，如果每个module同时又有相同的依赖、依赖相同的libary，（尤其是library）每次更新其版本都会是一件麻烦的事。因此集中管理依赖，对于一个proejct下多个module的情况来说是非常有用的。  
下面是项目的dependencies文件，可以看出有三个module：app、skin和data。还有一个名为CashBook的build.gradle文件是整个Project的dependencies。

![我是图，什么？我挂了？没事，不影响阅读](https://github.com/kangisme/img/raw/master/Android%20Studio%E5%A4%9Amodule.png)

在以project命名的build文件中加入如下内容：（举例而已，根据项目的依赖而不同）

	ext {
    	//http请求
    	okhttp = 'com.squareup.okhttp3:okhttp:3.9.1'
    	//okhttp支持包
    	okio = 'com.squareup.okio:okio:1.13.0'
    	//log日志工具
    	logger = 'com.orhanobut:logger:2.1.1'
    	//图片加载框架
    	picasso = 'com.squareup.picasso:picasso:2.5.2'
    	//事件总线
    	eventbus = "org.greenrobot:eventbus:3.0.0"
	}
说明：ext在这里相当于定义全局变量的意思

这时，在各个module里就可以利用上面定义的全局变量来快捷引用依赖了，如下：

    //图片加载框架
    compile rootProject.ext.picasso
    //log日志工具
    compile rootProject.ext.logger
    compile rootProject.ext.eventbus
输入完成后build一下，如果没有报错就表示通过了。