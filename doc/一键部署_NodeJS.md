#腾讯云实时音视频解决方案一键部署指引

腾讯云提供了全套技术文档和源码来帮助您快速构建一个音视频小程序，但是再好的源码和文档也有学习成本，为了尽快的能调试起来，我们还提供了一个快速一键部署服务（开发环境免费部署）：您只需轻点几下鼠标，就可以在自己的账号下获得一个音视频小程序，同时附送一台拥有独立域名的测试服务器，让您可以在 5 分钟内快速构建出自己的测试环境。
>注意：
测试中产生的云服务费将正常收取。
>相关云服务费详细介绍情参考 [购买指导](https://cloud.tencent.com/document/product/454/13649)

## 一、通过微信公众平台授权登录腾讯云

打开 [微信公众平台](https://mp.weixin.qq.com) 注册并登录小程序，保存小程序的WX_APPID 和WX_APPSECRET供后面使用，按如下步骤操作：

1. 单击左侧菜单栏中的【设置】。
2. 单击右侧 Tab 栏中的【开发者工具】。
3. 单击【腾讯云】，进入腾讯云工具页面，单击【开通】。
4. 使用小程序绑定的微信扫码即可将小程序授权给腾讯云，开通之后会自动进去腾讯云微信小程序控制台，显示开发环境已开通，此时可以进行后续操作。

> **注意：**
>
> 此时通过小程序开发者工具查看腾讯云状态并不会显示已开通，已开通状态会在第一次部署开发环境之后才会同步到微信开发者工具上。

![进入微信公众平台后台](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/wx_setting_1.png)

![开通腾讯云](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/wx_setting_2.png)

![腾讯云微信小程序控制台](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/wx_setting_3.png)

### 开通小程序类目与推拉流标签【重要】
出于政策和合规的考虑，微信暂时没有放开所有小程序对 &lt;live-pusher&gt; 和 &lt;live-player&gt; 标签的支持：

- 个人账号和企业账号的小程序暂时只开放如下表格中的类目：

<table>
<tr align="center">
<th width="200px">主类目</th>
<th width="700px">子类目</th>
</tr>
<tr align="center">
<td>【社交】</td>
<td>直播</td>
</tr>
<tr align="center">
<td>【教育】</td>
<td>在线教育</td>
</tr>
<tr align="center">
<td>【医疗】</td>
<td>互联网医院，公立医院</td>
</tr>
<tr align="center">
<td>【政务民生】</td>
<td>所有二级类目</td>
</tr>
<tr align="center">
<td>【金融】</td>
<td>基金、信托、保险、银行、证券/期货、非金融机构自营小额贷款、征信业务、消费金融</td>
</tr>
</table>

- 符合类目要求的小程序，需要在小程序管理后台的<font color='red'> “设置 - 接口设置” </font>中自助开通该组件权限，如下图所示：

![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/weixinset.png)

注意：如果以上设置都正确，但小程序依然不能正常工作，可能是微信内部的缓存没更新，请删除小程序并重启微信后，再进行尝试。

## 二、开通腾讯云服务
### 开通直播服务

#### 1. 申请开通视频直播服务
进入 [直播管理控制台](https://console.cloud.tencent.com/live)，如果服务还没有开通，则会有如下提示:
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/live_open.png)
点击申请开通，之后会进入腾讯云人工审核阶段，审核通过后即可开通。


#### 2. 配置直播码
直播服务开通后，进入【直播控制台】>【直播码接入】>【[接入配置](https://console.cloud.tencent.com/live/livecodemanage)】 完成相关配置，即可开启直播码服务：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/live_code.png)
点击【确定接入】按钮即可。

#### 3. 获取直播服务配置信息
从直播控制台获取`appID`、`bizid`、`pushSecretKey`，`APIKey`后面配置服务器会用到：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/live_config.png)

### 开通云通信服务
#### 1 申请开通云通讯服务
进入[云通讯管理控制台](https://console.cloud.tencent.com/avc)，如果还没有服务，直接点击**直接开通云通讯**按钮即可。新认证的腾讯云账号，云通讯的应用列表是空的，如下图：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/im_open.png)

点击**创建应用接入**按钮创建一个新的应用接入，即您要接入腾讯云IM通讯服务的App的名字，我们的测试应用名称叫做“RTMPRoom演示”，如下图所示：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/new_im.png)

点击确定按钮，之后就可以在应用列表中看到刚刚添加的项目了，如下图所示：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/im_list.png)

#### 2 配置独立模式
上图的列表中，右侧有一个**应用配置**按钮，点击这里进入下一步的配置工作，如下图所示。
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/im_config.png)

#### 3 获取云通讯服务配置信息
从云通信控制台获取`sdkAppID`、`accountType`、`administrator`、`privateKey`、`publicKey`，后面配置服务器会用到：
![](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/im_config_info.png)

从验证方式中下载公私钥，解压出来将private_key用文本编辑器打开，如：

```bash
-----BEGIN PRIVATE KEY-----
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-----END PRIVATE KEY-----
```

将其转换成字符串形式如下所示，后面在server配置文件中使用，<font color='red'>请注意每行后面要加入\r\n</font>：

```bash
"-----BEGIN PRIVATE KEY-----\r\n"+
"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\r\n"+
"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\r\n"+
"xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx\r\n"+
"-----END PRIVATE KEY-----\r\n"
```
public_key也采用同样的方式编辑，供后续使用。


## 三、安装微信开发者工具

下载并安装最新版本的[微信开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)，使用小程序绑定的微信号扫码登录开发者工具。

![微信开发者工具](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/wx_dev_tool2.png)

## 四、下载 Demo 和安装依赖

访问 [SDK+Demo](https://github.com/TencentVideoCloudMLVBDev/MiniProgram)，通过Github下载获取小程序 Demo 和后台源码。

## 五、上传和部署代码

1. 打开第四步安装的微信开发者工具，点击【小程序项目】按钮。
2. 输入小程序 AppID，项目目录选择上一步下载下来的代码目录，点击确定创建小程序项目。
3. 再次点击【确定】进入开发者工具。

> **注意：**
>
> 目录请选择 `MiniProgram-master` 根目录。包含有 `project.config.json`，请不要只选择 `wxlite` 目录！

![上传代码](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/wx_program_dir.png)

4 . 打开 Demo 代码中 `server` 目录下的 `config.js` 文件，按照上述步骤开通各项服务获取到的参数一一对应填写，并**保存**。
微信小程序参数：`appId`、`appSecret`
云直播服务参数：`appID`、`bizid`、`pushSecretKey`、`APIKey`
云通信服务参数：`sdkAppID`、`accountType`、`administrator`、`privateKey`、`publicKey`

![serverconfig](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/server_config.png)

5 . 点击界面右上角的【腾讯云】图标，在下拉的菜单栏中选择【上传测试代码】。

![上传按钮](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/upload_server_1.png)

6 . 选择【模块上传】并勾选全部选项，然后勾选【部署后自动安装依赖】，点击【确定】开始上传代码。

![选择模块](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/upload_server_2.png)

![上传成功]((https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/upload_server_3.png)

7 . 上传代码完成之后，点击右上角的【详情】按钮，接着选择【腾讯云状态】即可看到腾讯云自动分配给你的开发环境域名，完整复制（包括 `https://`）开发环境 request 域名，然后在编辑器中打开 `wxlite/config.js` 文件，将复制的域名填入 `serverUrl` 和 `roomServiceUrl` 中并保存，保存之后编辑器会自动编译小程序，左边的模拟器窗口即可实时显示出客户端的 Demo：

![查看开发域名](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/server_domain.png)

8 . 在模拟器中编译运行点击多人音视频进入，在右侧的console里面可以看到登录成功的log表示配置成功。

![登录测试](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/login_test.png)

9 . 请使用手机进行测试，直接扫描开发者工具预览生成的二维码进入，<font color='red'> 这里部署的后台是开发测试环境，一定要开启调试: </font>

![开启调试](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/xiaochengxutiaoshi.png)

或者直接扫描开发者工具远程调试生成的二维码开启真机调试
![开启调试](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/phone_debug.png)

<font color='red'> 注意：后台服务器部署的测试环境有效期为七天，如果还需要测试体验请重新部署后台。小程序访问域名有白名单限制，小程序开启调试就不会检查白名单，测试期间建议开启白名单，最后要发布的时候将域名配置到白名单里面，配置步骤请参考常见问题里"如何部署到正式环境"</font>

## 常见问题 FAQ
##### 1. 运行出错如何排查？
- 请修改`wxlite/config.js`中的url，使用默认的官方demo后台：`https://room.qcloud.com` ，直接运行小程序
- 请重新解压下载的demo直接运行小程序，默认就是官方demo后台
- 请返回第二步检查开通的小程序类目是否正确，推拉流标签在小程序控制台是否开启
- 使用官方demo后台运行可以，请参考此文档再重新部署一遍
- 依然不行可以提工单或客服电话（400-9100-100）联系我们

##### 2. 运行小程序进入多人音视频看不到画面?
- 请确认使用手机来运行，微信开发者工具内部的模拟器目前还不支持直接运行
- 请确认小程序基础库版本 wx.getSystemInfo 可以查询到该信息，1.7.0 以上的基础库才支持音视频能力。
- 请确认小程序所属的类目，由于监管要求，并非所有类目的小程序都开发了音视频能力，已支持的类目请参考 [DOC](https://cloud.tencent.com/document/product/454/13037)。
- 如有更多需求，或希望深度合作，可以提工单或客服电话（400-9100-100）联系我们。

##### 3. live-pusher、live-player标签使用及错误码参考
- [live-pusher&错误码](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-pusher.html)
- [live-player&错误码](https://mp.weixin.qq.com/debug/wxadoc/dev/component/live-player.html)
- [livePusherContext](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-live-pusher.html)
- [livePlayerContext](https://mp.weixin.qq.com/debug/wxadoc/dev/api/api-live-player.html)

##### 4. 如果需要上线或者部署正式环境怎么办？
- 请申请域名并做备案
- 请将服务端代码部署到申请的服务器上
- 请将业务server域名、RoomService域名及IM域名配置到小程序控制台request合法域名里面，其中IM域名为：`https://webim.tim.qq.com` 
![配置域名](https://github.com/TencentVideoCloudMLVBDev/MiniProgram/raw/master/image/domain_config.png)
 
