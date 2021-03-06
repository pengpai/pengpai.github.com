---
layout: default
title: iOS开发笔记-cocoaPods
---
### iOS项目中的依赖管理工具-cocoaPods

#### 背景介绍
现在,每个iOS project或多或少都会用到些第三方类库,比如大家熟悉的JSONKit,ASIHttpRequest等.当使用到的类库变得较多的时候,就会出现些依赖管理上的问题,比如循环依赖,版本控制等.

对于这类问题,其他语言早已经有自己比较成熟的解决方式,比如对于Java的maven.

那么,cocoaPods算是目前iOS平台上比较好用的一个.只要安裝好 CocoaPods，在自己的project项目里新建一份 Podfile 文件，在里面说明要使用哪些类库,什么版本等,剩下的一切都可以交给cocoaPods来搞定,过程相当轻松愉悦， 
	
#### 使用说明

1.	安装cocoaPods
	
	- 确保安装了gem(mac上默认安装了的)
	- 安装cocoaPods
	
			gem install cocoaPods
	 
	
2.	查找需要使用的类库

			pod search 'GPUImage'

3.	程序中引入类库
	
	- 在project根目录建立Podfile文件
	
			platform :ios
			pod 'ASIHTTPRequest', '~> 1.8.1'
			pod 'SBJson'
			pod 'EGOTableViewPullRefresh'
			pod 'DDPageControl'
			pod 'SOCKit'
			pod 'MBProgressHUD'
			pod 'SFHFKeychainUtils'
			pod 'RegexKitLite'
			pod 'UIDeviceAddition'
			
		说明:若不指定版本,则会引入最新版本的代码.
			
	- 初次使用,在该目录下执行
			
			pod install
			
	- 若不是初次使用,则执行
	
			pod update
			
	- project的相关配置
	
		- target -> build setting -> Search Path里.把cocoaPods的配置文件路径加进来		- 包括 LIBRARY_SEARCH_PATHS 和 USER_HEADER_SEARCH_PATHS 两个配置项,都要添加cocoaPods头文件配置的绝对路径
		- 若不配置的话,会出现库里的文件找不到的情况("symbol not found")
		
		
4.	自己的类库加入到cocoaPods仓库.

	1. 为自己的project添加一份specification即可
	
		Pod::Spec.new do |s|
  			s.name         = 'Reachability'
  			s.version      = '3.1.0'
  			s.license      =  :type => 'BSD' 
  			s.homepage     = 'https://github.com/tonymillion/Reachability'
  			s.authors      =  'Tony Million' => 'tonymillion@gmail.com' 
  			s.summary      = 'ARC and GCD Compatible Reachability Class for iOS and 								OS X. Drop in replacement for Apple Reachability.'
  			s.source       =  :git => 'https://github.com/tonymillion/R								eachability.git', :tag => 'v3.1.0' 
  			s.source_files = 'Reachability.h,m'
  			s.framework    = 'SystemConfiguration'
  			s.requires_arc = true
		end
	
	2. 在git上pull request,提交给cocoaPods团队审核
		
 5.	官方文档
 
 	http://docs.cocoapods.org/

	
#### 使用感受
优点不少,比如:

1. 循环依赖的问题可以有效解决.比如两个依赖到的类库本身也有依赖关系,cocoaPods可以帮你搞定
2. 依赖版本的问题等都能有效解决.
3. 以后你的git或svn只需要一份Podfile就可以,不用再把那些开源的代码也加到代码库里来.
4. 与直接拉第三方工程进来相比,不需要自己手动把test类和exmple去掉.

缺点呢,主要有二:

1. 对于我们自己有修改过的三方类库,比如ShareKit的config文件,是每个project个性化定制的,对于这类问题貌似还没有一个好的解决方法.
2. 依赖进来的个别第三方类库没有按照原有的项目包结构来组织,比如GPUImage.