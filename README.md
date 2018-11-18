# WebRTC

This project is only for studying and investigating the WebRTC.

### Download the WebRTC
  It is not easy to download the WebRTC in china because of the GFW. 
  要想下载WebRTC代码，首先得有一个FQ的工具，这里使用的是shadowsocks。
  1. 开启shadowsocks，使用全局代理模式。
  2. 官网：https://webrtc.org/. NativeCode-> Development 里面有步骤，需要仔细阅读。
  3. 首先装depot_tools. https://chromium.googlesource.com/chromium/tools/depot_tools.git 。然后将depot_tools添加到环境变量中去。
  4. gclient --version。 确保depot_tools下载，环境变量配置OK。
  5. 下一步正常来说是下载WebRTC源码了，windows在cmd模式下要配置代理
      set http_proxy=http://127.0.0.1:1080
      set https_proxy=http://127.0.0.1:1080 (注意看https的代理也是http://127.0.0.1:1080)
      配置git代理
      git config --global http.proxy 'http://127.0.0.1:1080'
      git config --global https.proxy 'http://127.0.0.1:1080' (和上面一样)
  6. 其他一些设置
     设置BOTO代理，解决download google storage失败问题： 
     set NO_AUTH_BOTO_CONFIG=$depot_tools_path/http_proxy.boto (其中$depot_tools_path是你的安装路径)
     设置不再次下载工具链： 
     set DEPOT_TOOLS_WIN_TOOLCHAIN=0 
     设置生成工程环境变量： 
     set GYP_GENERATORS=msvs-ninja,ninja 
     set GYP_MSVS_VERSION=2017 
  7. mkdir webrtc_checkout. cd webrtc_checkout. 然后执行fetch --nohooks webrtc.
     Note:正常来说进行了上面的配置就可以下载源码了，但有的时候也不行(我在MAC下OK，在windows底下不行)。windows底下可能还要配置：管理员模式进入cmd -> netsh-> winhttp -> set proxy 127.0.0.1:1080. 千万记住等OK了，要 reset proxy.
  8. 等下载完执行gclient sync。 后面可能下载不了某些Hooks，中断后，直接用网页下载好放到对应目录下，继续执行gclient runhooks就可以了。
     参见[https://blog.csdn.net/elesos/article/details/83785822]
  9. 下载完执行
      gn gen out/default/
      ninja -C out/default/ 就可以了
      编译完成后会发现peerconnection_client和peerconnecttion_server，以及会找到webrtc.lib等。后面可以用VS打开all.sln，进行调式运行了。至此，基本的开发环境算是搭好了。
