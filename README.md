# allinone-docker   https://www.youtube.com/watch?v=a6L-fDrJxag
中国IPTV直播源部署docker-live


#### 1、allinone指令：
<br>docker run -d --restart unless-stopped --net=host --privileged=true -p 35455:35455 --name allinone youshandefeiyang/allinone
<br>docker run -d --restart unless-stopped --net=host --privileged=true -p 35455:35455 --name allinone ghcr.io/dogwalkerg/allinone-docker/allinone

2、配置watchtower每天凌晨两点自动监听allinone镜像更新指令：
<br>docker run -d --name watchtower2 --restart unless-stopped -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower allinone -c --schedule "0 0 2 * * *"

上面2步就可以得到播放地址了：http://<IP替换>:35455/tv.m3u

.....................................................................................................

进阶优化：
3、直播源镜像 youshandefeiyang/allinone 项目的 m3u 进行重新分组格式化：
<br>docker run -d --restart=always -p 35456:35456 --name allinone_format yuexuangu/allinone_format:latest


<br>http://内网IP:35456/tv.php?h=allinoneIP&p=allinonePort&m=1&t=0

请求参数说明：
h 可选参数 allinone 项目部署的内外网 IP或域名（不能使用 127.0.0.1），默认值 = 请求的内网IP

p 可选参数 allinone 项目部署的端口, 默认值 = 35455

m 可选参数 是否对频道连接进行聚合，默认值 1=聚合（强烈推荐） 0=不聚合（仅推荐不支持聚合格式的壳子使用）

t 可选参数 输出格式 默认值 0=m3u， 1=text

@请求例子：
<br>http://<IP替换>:35456/tv.php

上面请求等同于
（这就是优化后的播放地址：）
<br>http://<IP替换>:35456/tv.php?h=<IP替换>&p=35455&m=1&t=0

（上面的<IP替换>包括括号都替换成你的设备IP）

上面替换完毕的网址就可以放到你的多平台播放器上打开了！享用吧

-----------------=================推送镜像到github方法：
ssh登陆   docker login ghcr.io -u dogwalkerg -p ghp_VTDEdk6HZETHtkJg347jMWiaQWrJTOKEN         #用户名+token
登陆后，把本地镜像改名为：
docker tag youshandefeiyang/allinone ghcr.io/dogwalkerg/allinone-docker/allinone               #前面为本地镜像，后面为改名后镜像名
Github仓库新建allinone-docker存储镜像仓库名后，直接推送成功，如下命令：
docker push ghcr.io/dogwalkerg/allinone-docker/allinone

# 创建 token
① 在 GitHub 任何页面的右上角，单击你的头像，然后单击 Settings。
<br>![图片描述](./1.png)
<br>② 在左侧边栏中，单击 Developer settings。
<br>![图片描述](./2.png)
<br>③ 在左侧边栏中，单击 Personal access tokens。
<br>![图片描述](./3.png)
<br>④ 单击 Generate new token。
<br>![图片描述](./5.png)
<br>⑥ 选择 read:packages 以下载容器镜像，读取元数据；选择 write:packages 以下载上传容器镜像，读取写入元数据；选择 delete:packages 以删除容器镜像。
<br>![图片描述](./4.png)
