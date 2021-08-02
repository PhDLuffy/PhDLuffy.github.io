---
layout:     post
title:      个人开发Apple Watch应用-“土味情话”
subtitle:   Apple Watch fetch网站API获取json数据
date:       2021-08-03
author:     PhDLuffy
header-img: img/Blog/blog-2021-08-03-01.jpg
music-id: 1368739803
catalog: true
tags:
    - 软件



---

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# 个人开发Apple Watch应用-“土味情话”



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**

#### 应用交互界面

应用交互界面非常简单，

一个VStack下面放一个“土味情话”标题Text和一个List，List下面放一个QinghuaView显示土味情话的内容。

List下面这个QinghuaView接收网络API的json数据并显示到一个Text里，同时在QinghuaView下的VStack下包含这个Text和一个Button，Button的作用是点击重新运行数据获取函数，刷新情话内容。



ContentView.swift

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading){
            Text("土味情话")
            List {
                QinghuaView().environmentObject(DataStore())
            }
        }    
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

```

QinghuaView.swift

```swift
var body: some View {
        VStack{
            Spacer()
            Text("\(qinghuaData.qinghuaneirong)")
            Button(""){
                self.lovelettergetData()
            }
        }  
    }
}

struct QinghuaView_Previews: PreviewProvider {
    static var previews: some View {
        QinghuaView()
    }
}
```

#### fetch网站API获取json数据

建立网站API的json数据结构体

举个栗子：

我的自建网站的API-qinghua，一个静态API，JSON数据结构如图1

结构为两层，一层是"code"，"msg"，"newslist"，我们需要的是newslist这一层下的信息，newslist下是一个content数组，所以我们的结构体应该这样写。

```swift
//第一个struct是第一层级的数据结构，我们只要newslist这一级的内容

struct LoveletterData: Decodable {
	var loveletter: [LoveletterFirstLayerData]
	
	enum CodingKeys: String, CodingKey {
		case lovetetter = "newslist"
	}
}

//第二个struct是第二层级的数据结构，newslist这一级里面只有一个content数组

struct LoveletterFirstLayerData: Decodable {
	var content: String
}
```

![图1](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210802220515.jpg)

JSON数据结构写完之后，我们写获取网络数据function

我们要获得API中的json数据

```swift
func lovelettergetData() {
	let urlString = "网站API的json数据请求地址"
	let url = URL(string: urlString)
	
	URLsession.shared.dataTask(with: url!) {data, _, error in
		DispatchQueue.main.async {
			var success = false
			if let loveletterdata = data {
				if let decodedloveletter = try?
					JSONDecoder().decode(LoveletterData.self, from: loveletterdata){
					self.qinghuaData.qinghuaneirong = 
						decodedloveletter.loveletter[Int.random(in: 0...20)].content
					success = true
					NSLog("\(success)")
				}
			}
			if !success {
					NSLog("webdata: no data found")
			}
		}
	}.resume()
}
```

我们获取到了网站JSON数据，赋值给了qinghuaneirong这个变量，我们需要将这个变量设为全局变量

新建一个swift文件：DataStore.swift文件

这里我们建立了一个类，DataStore，里面有一个全局变量qinghuaneirong

```swift
import Foundation

class DataStore: ObservableObject{
	@Published var qinghuaneirong: String = ""
}
```

我们在lovelettergetData()这个function里面建立一个变量，类型是DataStore

```swift
@environmentObject var qinghuaData:DataStore
```

这样的话，我们就能将API的json获取的内容赋予变量self.qinghuaData.qinghuaneirong了

在QinghuaView里，我们就可以引用这个变量的内容

```swift
Text("\(qinghuaData.qinghuaneirong)")
Button(""){
	self.lovelettergetData()
}
```

在ContentView中引用上面这个QinghuaView时候，还需要注意，我们要使用.environmentObject来引用，这样才能做到随时更新内容

```swift
QinghuaView().environmentObject(DataStore())
```

#### Complication复杂功能

显示界面非常简单，上方是标题“土味情话”，下方是我自己定制的显示内容，樱木花道的口头禅“我是天才”

> Complication复杂功能这一块，最初的设想是在表盘主界面随时间显示变化不同的情话内容，在数据交互上卡壳了一段时间，最近终于能把数据传进complication了，但是最终的显示效果并不好，内容字体会被apple watch系统放大，最多显示几个字，不能显示一整段内容，没法达到我想要的效果，应该是apple watchos7目前的系统限制，期望在未来能够开放相应的接口达到我最初想要的效果。

(A) complication复杂功能的模板内容设置

也就是当你在表盘复杂功能页面选择不同程序的complication默认的显示效果，在码代码之前，如图2需要先去这个位置，勾选需要用到的complication families，我目前只用到了Modular Large

![图2](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210802222324.jpg)

在complicationController.swift下

```swift
func getLocalizableSampleTemplate(for complication: CLKComplication, withHandler handler: @escaping (CLKComplicationTemplate?) -> Void) {
        
        switch complication.family {

           case .modularLarge:
               let template = CLKComplicationTemplateModularLargeTallBody()
               template.headerTextProvider = CLKSimpleTextProvider(text: "土味情话")
               template.bodyTextProvider = CLKSimpleTextProvider(text: "内容")
               handler(template)
               
           default:
               preconditionFailure("Complication family not supported")
           }
        
    }
```

getLocalizableSampleTemplate就是相应的模板，我们选择modularLarge这个family成员，template选择ModularLargeTallBody这个，具体的显示布局，苹果开发官网有详细的展示，这个比较适合我的程序。

headerTextProvider和bodyTextProvider是这个template所强制要求的，我们填入内容即可，text也要用它规定的CLKSimpleTextProvider来进行赋值。

(B) Complication表盘界面初始化

```swift
func getCurrentTimelineEntry(for complication: CLKComplication, withHandler handler: @escaping (CLKComplicationTimelineEntry?) -> Void) {
        
        let entry: CLKComplicationTimelineEntry
        let neirong: String
        
        neirong = "我是天才"
        
        switch complication.family {
           
               
           case .modularLarge:
               let template = CLKComplicationTemplateModularLargeTallBody()
               template.headerTextProvider = CLKSimpleTextProvider(text: "土味情话")
               template.bodyTextProvider = CLKSimpleTextProvider(text: neirong)
               entry = CLKComplicationTimelineEntry(date: Date(), complicationTemplate: template)
        
           default:
               preconditionFailure("Complication family not supported")
           }
           handler(entry)
    }
```

getCurrentTimelineEntry是程序在表盘的显示内容，本来我想在这里进行数据交互，让neirong能够随着程序按钮点击更换，发现这里的数据和程序里面的数据交互之间有问题，无法做到实时交互，查阅相关资料发现是watchos系统有限制，再加上我发现即使数据传过来，显示效果也不佳，所以就放弃了在这里进行数据交互，直接赋值，甚至我们直接将模板复制过来也可以。

> 这里记得在最后加一个handler(entry)
>
> 我尝试的数据传递方法是用UserDefaults，将变量付给这样的数据结构的变量即可，但是我发现数据更新不及时

(C) Complication刷新

在ExtensionDelegate.swift下

这里好像是一些能够调用Complication的东西，先建一个刷新Complication的function

```swift
func reloadActiveComplications() {
        let server = CLKComplicationServer.sharedInstance ()
     
        for complication in server.activeComplications ?? [] {
            server.reloadTimeline (for: complication)
        }
    }
```

这个function执行会刷新目前所有的Complication

在applicationDidFinishlaunching这个function下，我们执行reloadActiveComplication这个function

```swift
self.reloadActiveComplications()
```

这样当我们按数码表冠回到表盘的时候，系统会重新刷新所有Complication

#### 土味情话watch app最终的使用效果如下。

![表盘界面](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210720230232.jpg)

![app界面，点按内容可以自动刷新](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210720230247.jpg)

![点击刷新内容](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210720230252.jpg)

以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

