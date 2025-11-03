# VPS 自部署 4GTV 查看湾湾频道

## 注册账号

在 [https://www.4gtv.tv/signup.html](https://www.4gtv.tv/signup.html) 使用邮箱注册账号，记住**账号**和**密码**（后面会用到）

## VPS 部署 4GTV 教程

1. [点击购买VPS](https://my.racknerd.com/aff.php?aff=6200&pid=913)

2. 使用 [SSH工具](https://www.wangdu.site/software/bianchengkaifa/1263.html) 连接VPS

3. 安装宝塔面板

   ```sh
   if [ -f /usr/bin/curl ];then curl -sSO https://io.bt.sb/install/install_latest.sh;else wget -O install_latest.sh https://io.bt.sb/install/install_latest.sh;fi;bash install_latest.sh && rm -rf install_latest.sh
   ```

4. 登录宝塔面板，安装所需的环境，点击左侧 Docker 菜单，安装 Docker 环境

   ![宝塔面板-安装环境](https://cdn.wwkejishe.top/wp-cdn-02/2025/20250429134448730.webp)

5. 使用[SSH工具](https://www.wangdu.site/software/bianchengkaifa/1263.html)连接VPS，运行 [instituteiptv/4gtv-v1 - Docker Image | Docker Hub](https://hub.docker.com/r/instituteiptv/4gtv-v1)（**以下多行、单行任选其一，通用模式和host模式任选其一**）

   - 通用部署命令：（多行）

       ```sh
       docker run -d \
         --name=4gtv-v1 \
         -p 50007:5050 \
         --restart=always \
         instituteiptv/4gtv-v1:latest
       ```

   - （单行）：`docker run -d --name=4gtv-v1 -p 50007:5050 --restart=always instituteiptv/4gtv-v1:latest`
     - 访问地址：`http://主机IP:50007`

   - host模式部署命令：
   
       ```sh
       docker run -d \
         --name=4gtv-v1 \
         --network=host \
         --restart=always \
         instituteiptv/4gtv-v1:latest
       ```
       - 访问地址：`http://主机IP:5050`
   
6. 在 宝塔面板 - 安全 - 添加端口规则，根据上面模式放行 - 入站/出站 - 端口：`50007` 或 `5050`

7. VPS - SSH 安装 [nelvko/clash-for-linux-install: 😼 优雅地使用基于 clash/mihomo 的代理环境](https://github.com/nelvko/clash-for-linux-install)（常见问题：[FAQ · nelvko/clash-for-linux-install Wiki](https://github.com/nelvko/clash-for-linux-install/wiki/FAQ)），直接执行下面命令即可。

   ```sh
   git clone --branch master --depth 1 https://gh-proxy.com/https://github.com/nelvko/clash-for-linux-install.git \
     && cd clash-for-linux-install \
     && sudo bash install.sh
   ```

8. 第一次安装时需要输入代理订阅地址（**需要带台湾节点的**）。已经有代理节点的，输入自己的订阅地址；没有的话，订阅 [鹿语云](https://鹿语云.com/#/register?code=CZoLVxMH) 获取订阅链接。

9. 安装成功会给出`当前密钥`、`UI地址`

   ![clash-for-linux-install安装日志](https://cdn.wwkejishe.top/wp-cdn-02/2025/20251103141813840.webp)

10. 根据UI地址在 宝塔面板 - 安全 - 添加端口规则，根据上面模式放行 - 入站/出站 - 端口：`9090` 和 `7890`

11. 通过UI地址在浏览器打开，并在**代理**页面选择**台湾节点**

    - API Base Url：`http://主机IP:9090`
    - secret：第9步获取到的密钥

12. 使用 `echo $http_proxy` 来获取代理地址链接，用于后面输入到 4GTV

## 4GTV 设置

1. 通过上面地址访问 4GTV，并设置管理员账号和密码， 然后就是一般设置 - 账户设置：注册的账号、密码，存储设置，点击生成 Token 管理

   ![4GTV - 一般设置](https://cdn.wwkejishe.top/wp-cdn-02/2025/20251103142637453.webp)

2. 网络与代理设置，http代理和https代理输入上面**第 12 步**获取到的地址，服务器地址输入：上面4GTV部署的地址 `http://主机IP:50007 `或者 `http://主机IP:5050`

   ![网络与代理设置](https://cdn.wwkejishe.top/wp-cdn-02/2025/20251103142953734.webp)

3. 选择**线路**用于抓取播放地址

4. 设置**播放与流媒体代理设置**（播放类型、流媒体代理）：在 **播放清单档案 - 代理模式 - 检视**就是**m3u订阅链接**，并且走上面 clash 代理网络

5. 点击**手动更新**生成播放清单，输出以下内容代表成功。

   ```json
   {"filename":"playlist_reverse.m3u, playlist_proxy.m3u, playlist_stream_reverse.m3u, playlist_stream_proxy.m3u, playlist_direct.m3u","include_urls":true,"message":"\u64ad\u653e\u6e05\u55ae\u751f\u6210\u6210\u529f (\u5305\u542b\u64ad\u653e\u5730\u5740)","output_dir":"playlist","success":true}
   
   # 美观输出
   {
     "filename": "playlist_reverse.m3u, playlist_proxy.m3u, playlist_stream_reverse.m3u, playlist_stream_proxy.m3u, playlist_direct.m3u",
     "include_urls": true,
     "message": "播放清單生成成功 (包含播放地址)",
     "output_dir": "playlist",
     "success": true
   }
   ```

6. 使用**查看播放清单**或**线上预览**测试结果

## 播放器选择

1. Windows 播放器推荐 [CharmingTVBox](https://github.com/CharmingCheung/CharmingTVBox/releases)
2. 其他播放器推荐：[播放器合集](https://www.wangdu.site/software/av-read/339.html)

## 其他教程

- [2025年最新Docker本地部署4gtv教程：打造永不失联、稳定流畅的电视直播源！ - YouTube](https://www.youtube.com/watch?v=hkEg-KPPg1w)