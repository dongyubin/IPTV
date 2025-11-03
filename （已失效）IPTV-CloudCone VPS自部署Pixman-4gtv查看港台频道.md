# CloudCone VPS 自部署 pixman-4gtv 查看港台频道

1. 购买 [CloudCone VPS](https://app.cloudcone.com/vps/383/create?token=easter-25-ssd-vps-1&ref=11076)（ [CloudCone VPS备用链接](https://app.cloudcone.com.cn/vps/383/create?token=easter-25-ssd-vps-1&ref=11076)），这里选择的系统是：`Ubuntu 22.04 - x86_64`，hostname：`如果有自己的域名的话可以填写，也可以随便起个名字（Test、Tom、Jerry……）`，购买成功后CloudCone 会将 `VPS IP、账号、密码` 发送到邮箱，选择 [SSH客户端](https://www.wangdu.site/software/bianchengkaifa/1263.html) 进行连接。

   ![](https://cdn.wwkejishe.top/wp-cdn-02/2025/20250429133450026.webp)

2. 安装宝塔面板，全新安装成功后会给出`外网访问地址、账号、密码`（记录好信息，再进行第2行命令）
   ```sh
   # 1. 先全新安装
   wget -O install.sh http://io.bt.sb/install/install-ubuntu_6.0.sh && sudo bash install.sh
   # 2. 再激活企业版
   curl https://io.bt.sb/install/update_panel.sh|bash
   ```

   ![](https://cdn.wwkejishe.top/wp-cdn-02/2025/20250429133455691.webp)

3. 登录宝塔面板，安装所需的环境，点击左侧Docker菜单，安装 Docker 环境

   ![宝塔面板-安装环境](https://cdn.wwkejishe.top/wp-cdn-02/2025/20250429134448730.webp)

4. 使用[SSH工具](https://www.wangdu.site/software/bianchengkaifa/1263.html)连接VPS，运行 [mybtjson/pixman-4gtv - Docker Image | Docker Hub](https://hub.docker.com/r/mybtjson/pixman-4gtv)

5. 先去 [https://app.alice.ws⁠](https://app.alice.ws/) 注册账号，然后把你的 vps 的 IP 添加到他官网提供的 dns 解锁中（公共DNS - 公共流媒体DNS - 新用户要申请，等待审核通过即可）， 添加成功之后会给你一个解锁HK和TW的IP，把下面`--dns=8.8.8.8`中的`8.8.8.8`替换成`他给你提供的IP`（下面3个版本可以先安装一个试试，能播放就不用安装另一个，我这里0.0.3可以播放，0.0.5无法播放，最好是将 `50007` 换为别的数字端口，防止被扫）。

   ```
   # 0.0.3版本
   docker run -d --name=pixman-4gtv -p 50007:5000  --dns=8.8.8.8 --restart=always mybtjson/pixman-4gtv:0.0.3
   # 0.0.5版本
   docker run -d --name=pixman-4gtv -p 50007:5000  --dns=8.8.8.8 --restart=always mybtjson/pixman-4gtv:0.0.5
   # 0.0.1版本，稳定，部署完毕需要台湾家宽节点播放
   docker run -d --name=woshishabi -p 50007:5000 --restart=always mybtjson/pixman-4gtv:0.0.1
   ```

6. 运行成功后，通过下面订阅地址，访问 txt、m3u 2种格式订阅连接：

   ```bash
    http://ip:port/?type=txt
    http://ip:port/?type=m3u
   ```

   