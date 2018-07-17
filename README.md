# NumberOne
Web MI 网页即时通讯
后期将会将修改了源码部分的代码还原，方便大家升级layim。新添加的功能将全部移至socket.js，不会改动layim.js，并且会写一个文档，方便在基础上进行开发 

本webim是基于 layim 和环信webim3.X开发而成的，本项目仅供学习使用，使用前请先到layim官网获取layim的授权许可 。目前已完成的功能有： 1.好友/群内的文字、表情、图片、文件 在线/离线消息发送和接收。 2.查看群员列表。 3.面板内快速查找。 4.面板右键自定义事件 5.修改签名 6.自定义上传背景皮肤 7.搜索好友/群 8.添加好友/群 9.新建群 10.消息盒子展示 11.查看/修改个人信息 12.实时获取好友在线状态 13.挤下线提醒 14.文件储存在七牛云 15.增删改 好友/好友分组 16.群管理（增删管理员/修改群名片/单个群员禁言解除禁言/踢人）

需要手动修改的地方包括


1 static/js/webim.config.js 环信配置文件appkey，需要先到环信注册添加应用获取。 2 class/config.php 配置文件 3 如果想在本地运行demo，但是又没有layim.js文件，想试用后再去购买授权，那么请到test.guoshanchina.com登录后通过查看页面源码找到layim.js文件下载并保存到对应文件位置即可。 ##说明 1）因为考虑到需要和app之间进行通信（表情，图片等），环信的表情定义为[/:u]类似的字符而layim则为face[/:u]的字符，为了同时满足两种情况，修改了layim.js的表情相关代码，请知晓 2） 自定义右键的删除功能借鉴的是layim的删除历史会话 3） 自定义上传皮肤功能，在layim.js添加了一个setSkinByUser的方法，并修改了皮肤寻则模版，对应的上传路径是class/doAction.php?action=uploadSkin 4） 面板内的搜索好友功能，修改了layim.js的search方法

体验地址：test.guoshanchina.com

体验帐号：

用户名：911088 密码：123456

用户名：1570855 密码：123456

用户名：1570845 密码：123456

用户名：911058 密码：123456

用户名：910992 密码：123456

用户名：911067 密码：123456

用户名：911100 密码：123456

用户名：911085 密码：123456

开始之前，你要了解layui扩展第三方插件的方法，然后在环信注册帐号并创建应用。

layui绑定扩展

     layui.config({
         base: 'static/js/'
     }).extend({
         socket: 'socket',
     });
layim,socket初始化

     layui.use(['layim', 'jquery', 'socket'], function (layim, socket) {
         var $ = layui.jquery;
         var socket = layui.socket;
         var token = $('body').data('token');
         var rykey = $('body').data('rykey');           
         socket.config({
             user: token,
             pwd: rykey ,
             layim: layim,
         });

         layim.config({
             init: {
                 url: 'class/doAction.php?action=get_user_data', data: {}
             },
             //获取群成员
             members: {
                 url: 'class/doAction.php?action=groupMembers', data: {}
             }
             //上传图片接口
             , uploadImage: {
                 url: 'class/doAction.php?action=uploadImage' //（返回的数据格式见下文）
                 , type: '' //默认post
             }
             //上传文件接口
             , uploadFile: {
                 url: 'class/doAction.php?action=uploadFile' //
                 , type: '' //默认post
             }
             //自定义皮肤
             ,uploadSkin: {
                 url: 'class/doAction.php?action=uploadSkin'
                 , type: 'post' //默认post
             }                    
             ,systemSkin: {//选择系统皮肤
                 url: 'class/doAction.php?action=systemSkin'
                 , type: 'post' //默认post
             }
             , isAudio: false //开启聊天工具栏音频
             , isVideo: false //开启聊天工具栏视频

             //扩展工具栏
             // , tool: [{
             //         alias: 'code'
             //         , title: '代码'
             //         , icon: '&#xe64e;'
             //     }]
             ,title: '我的webim' 
             ,copyright:true
             , initSkin: '1.jpg' //1-5 设置初始背景
             , notice: true //是否开启桌面消息提醒，默认false
             , msgbox: layui.cache.dir + 'css/modules/layim/html/msgbox.html' //消息盒子页面地址，若不开启，剔除该项即可
             , find: layui.cache.dir + 'css/modules/layim/html/find.html' //发现页面地址，若不开启，剔除该项即可
             , chatLog: layui.cache.dir + 'css/modules/layim/html/chatLog.html' //聊天记录页面地址，若不开启，剔除该项即可
         });  
     });

【注意】github 上传的代码已去除layim.js，所以下载代码后请在你获取到 layim.js授权后将 layim.js 拖进 static / layui / lay / modules / 文件夹内方可运行。
