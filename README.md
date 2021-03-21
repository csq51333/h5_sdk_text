# AIHelp H5 接入文档 
## 1. 引入 

**页面初始化（必须在页面初始化阶段调用）**<br />
**甲方有义务按照乙方接入文档说明的正常接入方式和调用方式使用乙方服务，如甲方通过技术手段影响乙方计费，乙方有权在通知甲方的同时立即单方面终止服务，并要求甲方承担责任。**<br />

>在页面引入js文件

	https://aihelp.net/aihelph5/js/aihelp.js

## 2. 创建初始化参数
在本地js文件中创建初始参数[object] appId,appName,domain,mode,userId,userName,language,userTags,customData,welcomeMessage等参数。

>示例:

	var elva_conf = {
		appId: `${appId}`,
		appName: `${appName}`,
		domain: `${domain}`,
		mode: `showFAQSection`,
		sectionId：`${sectionId}`,
		conversationInten: `${hsTags}`,
		userId: `${userUid}`,
		userName: `${userName}`,
		appName: `${appName}`,
		language: `${language}`,
		userTags: `${userTags}`,
		customData: `${customData}`,
		conversationInten: 1,
		welcomeMessage: "您好，有什么问题可以帮您",
		alwaysShowHumanSupportButtonInBotPage: true
	}  

**说明:**<br /> 
**必传项**：<br />
appId: 不同平台的id,此处需使用web的appid.<br />
appKey: 应用名.<br />
domain：域名.<br />
conversationInten: 设置为“1”时，展示机器人客诉页面，设置“2”时展示人工客诉页面.<br />
alwaysShowHumanSupportButtonInBotPage: 设置为“true”时，机器人客诉页面显示人工入口，设置“2”时展示人工客诉页面.<br />
welcomeMessage：人工客服自定义欢迎语.<br />
mode: 进入模块类型：根据传值对应进入不同页面：<br />

> **mode:"showConversation"**:  默认进入机器人客服页面，设置conversationIntent为“2”，进入人工客服页面<br />
> **mode:"showAllFAQSections"**:  进入所有FAQ分类列表页面<br />
> **mode:"showFAQSection"**:  单独展示某个 FAQ 分类，此时sectionId为必传项<br />
> **mode:"showSingleFAQ"**:  展示某个特定的 FAQ 问题，faqId为必传项<br />
> **mode:"showOperation"**:  进入运营页面<br />
<br />

**选传项**：<br />
userId: 用户唯一标识. 选传项,AIHelp会优先使用您所传的uid,不可以是 0 或 -1,若是uid为空,AIHelp会根据用户的设备与浏览器生成唯一id作为用户uid.<br />
userName: 用户名. 选传项,AIHelp会优先使用您所传的userName,AIHelp会将所有未传userName的用户命名为Unknown_User.<br />
appName: 自定义APP名称.<br />
language: 语言. 选传项,AIHelp会优先使用您所传的language设置SDK的默认语言；如果不传，则使用当前设备语言初始化 SDK.<br />
userTags: 用户标签. 选传项,会将所传标签在AIHelp客服后台客诉中显示.多个标签之间以“,”分隔<br />
customData: 自定义数据. 选传项,JSON字符串，格式：{“key”:“value”, “key”:“value”}.<br />

**特殊项**：<br />
sectionId: 当mode为“showFAQSection”时，sectionId为必传项.<br />
faqId: 当mode为“showSingleFAQ”时，faqId为必传项.<br />

    
**注： appId** 请使用注册邮箱登录 [AIHelp 后台](https://console.aihelp.net/elva)。在Settings菜单Applications页面查看。初次使用，请先登录[AIHelp 官网](http://aihelp.net/index.html)自助注册。<br />

## 3.	调用elvah5.init()传入初始化参数
>示例:

	if (typeof elvah5 !== undefined) { 
		elvah5.init(elva_conf)     
	} 
  
## 4.	调用elvah5.show() 函数
在页面UI事件响应函数中，调用elvah5.show()弹出elva盒子
> 示例:

	btn.onclick = function () { 
		elvah5.show()  
	}

## 5.	使用window.addEventListener 函数接收推送 (非必选)
通过接收AIHelp的推送来给玩家推送消息
> 示例:

	window.addEventListener("message", function(MessageEvent){
    var origin = event.origin || event.originalEvent.origin;
    if (window.Notification&&origin==='https://aihelp.net') {
      if(window.Notification && Notification.permission !== "denied") {
        Notification.requestPermission(function(status) {
          var n = new Notification(`您有一条新的消息`,{
            icon: "https://cdn.aihelp.net/img/logo_small.png",
            tag: "AIHelp.net",
            renotify: true,
            body: MessageEvent.data.msg?MessageEvent.data.msg:''
          }); 
          n.onclick = function(event) {
            event.preventDefault(); 
            window.focus()
          }
        });
      }
    }
    }, false);
  

我们现在的推送规则是:当客服回复给玩家消息就会推送,具体什么时候给玩家提示可以在接入的时候根据具体场景或需求自行设计

## 6.	自定义弹出elva盒子的样式
> 示例:

	.elvaBox {    //聊天界面
		width: 375px;
		height: 500px;
		position: fixed;
		right: 1rem;
		bottom: 4rem;
	}


	.close {   //关闭按钮（PC端网页）
		position: absolute;
		right: 10px;
		top: 10px;
		width: 30px;
		height: 30px;
		color: #fff;
		background: #f9c633;
		border-radius: 25px;
		cursor: pointer;
	}
	

	.close:before {
		position: absolute;
		content: '';
		width: 20px;
		height: 2px;
		background: #fff;
		transform: rotate(45deg);
		top: 14px;
		left: 6px;
	}

	.close:after {
		content: '';
		position: absolute;
		width: 20px;
		height: 2px;
		background: #fff;
		transform: rotate(-45deg);
		top: 14px;
		left: 6px;
	}

PC端效果:

![PC效果图](https://github.com/AIHELP-NET/Pictures/blob/master/AIHelp-H5-on-PC(1).jpg "h5")

移动端效果:       **更多移动端接入方式,**[请戳这里](https://github.com/AI-HELP/H5-access-stable/blob/master/more_reference_CN.md)

![移动端效果图](https://github.com/AIHELP-NET/Pictures/blob/master/AIHelp-H5-on-mobile(1).jpg "h5")

代码示例：

![h5](https://github.com/AI-HELP/H5-access-stable/blob/master/AIHelp-H5-on-mobile(2).png "h5")
