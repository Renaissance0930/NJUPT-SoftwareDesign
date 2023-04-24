# NJUPT-SoftwareDesign
2020级通信工程软件设计-泛在物联网设计记录
=======
概述：
------
本次软件设计课题可以选择自拟题目，在众多高算法要求的题目中，实在难以施展拳脚（通信工程专业对算法和编程这一块基本没有教学），故选择自拟题目，在与老师协商讨论后，选择了智能家居这一块相对高功能要求，低算法要求的板块进行软件设计
-------
* 设计目标如下：
![](https://github.com/Renaissance0930/NJUPT-SoftwareDesign/blob/main/%E8%AF%BE%E9%A2%98%E7%9B%AE%E6%A0%87.png)
* 材料汇总：<br>
服务器：Jetson Xavier NX（能连网能跑docker的嵌入式板子都可以，云服务器也行，树莓派最好，后面会讲到，最小硬件支持：2GB RAM，32GB ROM，2核CPU）<br>
单片机：esp8266<br>
传感器: <br>
光照传感器：TSL2591<br>
温湿度气压传感器：BME280<br>
紫外线传感器：LTR390<br>
可燃气体传感器：SGP30<br>
步进电机：28BYJ48<br>
  
* 结构介绍：<br>
本课题要求：整个系统使用传感器+单片机+必要的终端和后台来构建，我们需要一台服务器来作为整个系统的数据存放和前后端构建，单片机和传感器作为信息采集和自动化控制的驱动部分
  * 服务器部分：<br>
  由于软件设计周只有两周时间，前后端的设计开发必然会浪费大量时间（别的组都是3个人，我们组只有两个人，人手很紧张，加上我们专业也不太会写代码，自学又要花很多时间），这里我们选择走捷径，使用开源的home assistant来进行前后端的管理，由于题目中不允许使用计算机，我手上正好有一块之前学GPU编程买的的嵌入式开发板Jetson Xavier NX，使用dockers搭建服务器部分，加上portainer的图形化管理面板，维护起来也较为方便。
  * 单片机部分：<br>
  由于使用home assistant作为系统中枢，单片机的选择尤为重要，加上我这种是穷大学生，在材料的性价比上也有要求，百般查询，确定esp8266作为实验的单片机部分，可以使用esphome直接连接到home assistant中，只接在esphome服务器中编辑yaml配置文件，服务器会自动编译生成固件并通过WiFi以OTA的形式烧录到esp8266上，并且可以在云端查看日志报告，免去了连线冗杂的麻烦。
  * 传感器部分：<br>
  由于esphome收录的传感器有限，根据实验要求选择的传感器型号，能基本满足实验的需求的同时也能在esphome中简单配置。
  * 关于步进电机：<br>
  在要求中有对窗帘的自动控制，这是本实验的一个难点，从步进电机驱动到yaml配置文件的编写，再到根据光照强度自动控制窗帘的开闭，都需要多设备共同协作。
  * 语音控制部分：
  这里我也走了一个捷径，在所有设备接入苹果生态后，一句“嘿，Siri”就能控制所有设备。
* 服务器搭建
  * Home assistant服务端的搭建<br>
  Home assistant[官网](https://www.home-assistant.io/)在官网可以了解到home assistant的用途，简单来说就是一个智慧家庭的中枢，可以将米家，苹果的homekit等智能设备全部集成到一起的一个终端，可以实现米家的设备在苹果的家庭中显示，也能将一些未经过苹果官方认证的设备（比如本课题的诸多传感器）加入到苹果的生态中。<br>
  docker的安装不再赘述，自行百度即可<br>
  home assistant的docker镜像安装：
  ```
  docker run -d \
  --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -e TZ=MY_TIME_ZONE \
  -v /PATH_TO_YOUR_CONFIG:/config \
  --network=host \
  ghcr.io/home-assistant/home-assistant:stable
  ```
  安装好后在服务器内网中输入：`服务器IP:8123`即可进入管理后台<br>
  根据提示进行初始的账号密码配置即可<br>
  Home assistant服务端界面：<br>
  ![](https://github.com/Renaissance0930/NJUPT-SoftwareDesign/blob/main/Homeassistant%E7%95%8C%E9%9D%A2.jpg)
  * ESPhome服务端的搭建<br>
  ESPhome作为home assistant和esp8266开发板之间的桥梁，在固件的编译和后期Debug上都起着关键的作用。<br>
  ESPhome服务端安装：
  1、如果你的home assistant是supervisor版或os版，可以在自带的商店内搜索`ESPhome`下载安装。<br>
  2、如果你的home assistant是core版，很不幸，这个版本的home assistant仅包含必要的组件，不支持商店安装（比如我），这里使用docker安装或安装包安装<br>
  docker安装：<br>
  ```
  docker pull ghcr.io/esphome/esphome
  ```
  即可<br>
  ESPhome服务端界面：<br>
  ![](https://github.com/Renaissance0930/NJUPT-SoftwareDesign/blob/main/ESPhome%E7%95%8C%E9%9D%A2.jpg)

实验周刚进行一半，很多传感器材料买了快递还没到，过两天完善了继续更
=========
  
