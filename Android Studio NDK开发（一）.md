## Android Studio NDK开发（一） ##
### 1.开发准备 ###
采用最新的开发工具，到目前为止（2018/1/3），各个工具的最新版本如下： </br>
Android studio 3.0.1</br>
CMake 3.6.4111459</br>
LLDB 3.0</br>
NDK 16.1.4479499</br>

下面分别对这些工具进行简单的介绍：</br>
#### Android Studio 3.0.1 ####
新版本的Android studio 提升编译速度，添加Kotlin支持，还有一些新的工具如Profiler，等等，还是值得升级的。
### CMake ###
在Android studio2.2以上版本，将默认采用CMake来编译C和C++代码。CMake是一款开源的跨平台自动化构建系统，它通过CMakeLists.txt来声明构建的行为，控制整个编译流程，在接下来的NDK开发中将会使用它配合Gradle来进行相关开发。