该镜像是在alpine最新版本基础上编译安装ffmpeg 4.2.2版本, 包含jre, 接着安装graphicsmagic  1.35版本。

graphicMagic：主要用来获取图片格式及长款高，目前用于图片上传接口获取webp、apgn等非标准格式图片的长宽高。具体方法在CMS-SHIRO-SERVER的fileutil类中getImageBasicInfo方法。

http://www.graphicsmagick.org/
