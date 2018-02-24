问题：
在 WordPress 中使用裁剪图片功能时，出现：“在裁剪您的图像时发生了错误” 或者 “There has been an error cropping your image.”

原因：
缺少 PHP GD 库

Ubuntu下运行：

sudo apt-get install php5-gd

or

sudo apt-get install php7.0-gd

CentOS下运行：

yum install php-gd

安装完以后重启 Apache 或 Nginx 即可