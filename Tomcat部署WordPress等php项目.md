Tomcat一般用来部署Java项目，但我想装个WordPress玩玩，而WordPress是php项目，起初以为不行，但总有先行者已经解决了我目前所遇到的问题，这次也不意外。特此记录一下：


1. 我们把php项目当做一个web项目放在webapps下，并在里面建WEB-INF\lib目录。例如: webapps\php\WEB-INF\lib



1. 从[http://quercus.caucho.com/](http://quercus.caucho.com/ "http://quercus.caucho.com/")。下载quercus-4.0.39.war，提取里面的web.xml放到WEB-INF下面，提取里面的jar放到WEB-INF\lib下面。



1. 重启tomcat，centos上在Tomcat的bin目录下，先停止再打开即可
2. 将index.php放入php目录下（默认路径，可在web.xml中改）

3. 然后浏览器访问xxx/php就可以了