# Dockerfile
```Dockerfile
FROM ubuntu

RUN apt update && apt upgrade -y && apt install default-jre -y && apt install ffmpeg -y && apt-get install graphicsmagick -y && apt install npm -y

RUN npm i -g @squoosh/cli

# download from https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-1.3.0-linux-x86-64.tar.gz
ADD webpmux /usr/local/bin
ADD cwebp /usr/local/bin
ADD anim_dump /usr/local/bin
ADD img2webp /usr/local/bin

# download from  https://jaist.dl.sourceforge.net/project/apngdis/2.9/apngdis-2.9-bin-macos.zip
ADD apngdis /usr/local/bin
```
# 创建镜像
```shell
docker build -t public.ecr.aws/s6f9i9p7/openjre11-ffmpeg-graphicsmagick-imgcompress .
```
# 登录ecr
```shell
export AWS_ACCESS_KEY_ID=xxx && export AWS_SECRET_ACCESS_KEY=xxx && export AWS_DEFAULT_REGION=ap-southeast-1
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws
docker push public.ecr.aws/s6f9i9p7/openjre11-ffmpeg-graphicsmagick-imgcompress
```
