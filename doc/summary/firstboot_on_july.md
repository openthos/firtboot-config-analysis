# firstboot_config 功能界面需求与设计实现文档
内容：

- 项目简介
- 功能需求
- 存在问题
- 项目进展
- 设计实现

# 项目简介

本项目属于openthos项目的一部分，提供 [Openthos](https://github.com/openthos/openthos/wiki) 对原第一次启动设计实现以及功能扩展。

##　当前开发人员 (20170316-20170415)
刘晓旭 周怡洁

##　当前开发人员 (20161101-20161130)
李兵 周怡洁

##　当前开发人员 (20160801-20160831)
李兵

## 功能需求

|完成|描述|模块|完成度|
|---|---|---|---|
|√| 初版界面实现（语言选择，网络，openthos注册，主机名设置）|界面|100%|
|√| 语言选择功能|功能|100%|
|√| openthos注册功能|功能|100%|
|√| openthos验证功能|功能|100%|
|√| 主机名用户名设置功能|功能|100%|
|√| 锁屏密码功能实现|功能|100%|
|√| 添加有线网功能|功能|100%|
|√| 添加有线网界面|界面|100%|
|√| 设置第一次启动网络优先级|功能|100%|
|√| 根据最新ＵＩ实现界面改动（语言选择，网络，openthos注册，主机名设置，结束页面）|界面|100%|

## ８月任务计划

|时间节点|任务计划|
|---|---|
|2016.8.1-2016.8.15|根据UI图以及测试建议修改完善界面|
|2016.8.16-2016.8.31|完善第一次启动过程中剩余问题以及未实现的锁屏功能|

## ８月任务计划(20160820-20160902)

|时间节点|任务计划|
|---|---|
|2016.8.20-2016.8.26|根据UI图以及测试建议修改完善界面,使用新的html注册界面完成openthos注册页面|
|2016.8.30-以后|实现锁屏密码设置功能|

## 11月任务计划(20161101-20161130)

|时间节点|任务计划|
|---|---|
|2016.11.01-2016.11.30|周怡洁(李兵辅助)：</br>1.剩余于bug解决（442/441）</br>2.锁屏密码设置（同settings）</br>3.多用户问题（后期讨论）</br>|

## 2017年03、04月任务计划(20170316-20170415)

|时间节点|任务计划|
|---|---|
|2017.03.16-2017.04.15|1.剩余bug解决</br>2.额外增加的feature|

# 存在问题

| 简述 | 类别 | 备注
|---|---|---|
|选择语言-中英文切换时出现黑屏闪一下|bug|参考设置语言切换的处理方式|
|引导界面按钮增加键盘快捷键支持|功能|查看需求列表确认是否增加|

# 项目进展

- 2017/03/16-2017/04/15
  * 刘晓旭
    * 截止到目前，第一次启动引导界面的需求大部分都已经实现，剩下的主要工作如下：
      * 相关的bug修改
      * 对于提出的一些增加feature的建议，同测试沟通，确认是否需要增加feature
      
- 2016/06/16-2016/06/30
  * 李兵
    * 着手调查第一次启动相关代码，添加剩余页面，包括注册页面验证页面以及用户名设置界面，并完善相关简单功能

- 2016/07/01-2016/07/15
  * 李兵
    * 实现以太网功能追加，与后台人员对接完成openthos注册与验证功能，调研实现用户名、主机名的设置，但并不完善，仍剩余部分问题。
 
- 2016/07/16-2016-/07/28
  * 李兵
    * 着手解决剩余问题，并开始按ＵＩ样式实现第一次启动界面修改，同时实现了第一次启动network界面中有线网界面的追加

# 设计实现

设置向导界面基本就是个应用，我会针对其中重要的部分进行说明

## 整体结构构造
###第一次启动相关设置，流程界面六个
- 语言选择界面（默认选择中文，可切换）SetupWizardActivity.java 
- 网络链接界面（直接是用的settings中的代码，具体内容需追踪到settings中调查）
- openthosID验证界面（需要和后台数据交互）OpenthosIDSetupActivity.java
- 用户名／密码注册界面（锁屏密码功能还为完善）UserSetupActivity.java
- 最后的完成界面FinishPagerActivity.java
- 在验证时会涉及到注册界面，此时注册界面直接走的网址，需要用WebView包裹，由于不能使用浏览器，所以自己添加了一个OtoUserAppWizard 应用，去处理网络注册的问题　OpenthosIDRegisterActivity.java

## 涉及到的功能点
- 语言切换（代码可见，无可分析点）
- 网络优先级(一切实现均在Settings中，分析困难度会大一些)
- openthos验证数据交互（设计到跟java后台工程是交互，此时设计了Utils类，用于与后台交互数据）
- 用户名／主机名输入（分析难度较大，设计文件读写，不可更改字段等问题）



## 针对于遇到的困难问题解决思路及方式
- 语言切换遇到问题
  - 当语言切换后，点击计入下一页时，屏幕会黑一下，而在设置界面进行语言切换时，不存在此问题，所以可以参考设置界面语言切换方式去修改此问题。
  - 曾经出现过每次切换语言后系统会崩掉，导致整个statusbar隐藏掉，原因最初的实现没切换一次语言，都
    ```
    new Handler().post(new Runnable() {
                            public void run() {
                                updateLocale(mCurrentLocale);
                            }
                        });
    ```
    调用它导致
- 网络优先级
　在androidX86中加入有线，在二者同事存在时区分优先级，默认二者优先级相同，在同事存在时无法保证有线优先级高于无线，将优先级设置值高于１００,即可解决此问题；但针对于具体应用中的优先级问题扔没有很好的处理解决
- openthos验证数据交互
　主要设计到，将拿到的数据通过request请求发给服务端，在服务端处理完毕后返回给客户端，客户端根据返回值在判断成功与否，基本的交互流程
- 用户名／主机名输入
　修给此两项内容，需要修改system/build.prop文件中的ro.build.host/name属性，但此属性是不可修改的，在程序中会有校验，若key值为ro.build开头的则禁止操作，所以我们增加了新的工具类，用于提升build.prop文件的读写权限，先将其改为可读写的，将参数值写入后，在将文件权限恢复原状。但通过SystemProperies去得的值，需要重启后才会好使，无法立即写入中间层，我们想要立即生效只好再此直接读取该build文件


