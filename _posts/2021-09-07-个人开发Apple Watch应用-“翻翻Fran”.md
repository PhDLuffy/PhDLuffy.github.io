---
layout:     post
title:      个人开发Apple Watch应用-“翻翻Fran”
subtitle:   Apple Watch个人程序-日语单词小应用
date:       2021-09-07
author:     PhDLuffy
header-img: img/Blog/blog-2021-09-07-01.jpg
music-id: 1380239916
catalog: true
tags:
    - 软件



---

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210519000143.gif)

# 个人开发Apple Watch应用-“翻翻Fran”



> **既然没有人愿意站出来唱唱反调，那我来吧。**
>
> **------ 卢娜·洛夫古德**



#### 开发准备：

升级MacOS系统到最新的正式版，升级Xcode到最新的正式版本

升级MacOS系统到最新的正式版，升级Xcode到最新的正式版本

升级MacOS系统到最新的正式版，升级Xcode到最新的正式版本

重要的事情说3遍。

开发之前，一定要设置WatchOS Deployment Target为最低watchOS 7.1

watchOS的大版本系统之间的差异非常大，为了避免不必要的麻烦，建议使用最新的watchOS

> 因为我的watch SE目前的版本是watchOS7.1，所以我选择了最低为watchOS7.1



#### 应用交互界面

应用交互界面非常简单

一个VStack下面放一个标题Text和一个List，List下面放一个DanciView显示单词的内容。

VStack下面的Text和List下面这个DanciView接收网络API的json数据并显示，同时在DanciView下的VStack下包含3个Text（分别显示日语，罗马字，翻译）和一个Button，Button的作用是点击重新运行数据获取函数，刷新单词内容。

![主程序界面](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210907223000.JPG)

ContentView.swift

```swift
import SwiftUI

struct ContentView: View {
    
    @ObservedObject var danciData = DataStore()
    
    var body: some View {
        VStack(alignment: .leading){
            Text("\(danciData.language)")
            List {
                DanciView()
            }
        }.environmentObject(danciData)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

DanciView.swift

```swift
var body: some View {
        VStack(){
            Spacer()
            Text("\(danciData.riyu)")
            Spacer()
            Text("\(danciData.luomazi)")
            Spacer()
            Text("\(danciData.fanyi)")
            Button(""){
                self.dancigetData()
            }
        }
    }
}

struct DanciView_Previews: PreviewProvider {
    static var previews: some View {
        DanciView()
    }
}
```



#### 建立网站API的json数据结构体

举个栗子：

我的自建网站的API-fran，一个静态API，JSON数据结构如图

结构为两层，第一层是"languageData"，"riyuData"，"luomaziData"，"fanyiData"，第二层为数据或数组。

"languageData"下面就一个数据，显示语言分类，这个数据同时被ContentView中的标题Text接收并显示。

"riyuData"，"luomaziData"，"fanyiData"，下面分别是各自的内容数组，每组对应一个日语单词的平假名，罗马字和中文翻译。

综上，获取API的json内容的结构体应该这样写。

```swift
class DanciGetData{
    struct DanciData: Decodable {
        var languageData: LanguageData
        var riyuData: [RiyuData]
        var luomaziData: [LuomaziData]
        var fanyiData: [FanyiData]
    }

    struct LanguageData: Decodable {
        var language:String
    }

    struct RiyuData: Decodable {
        var riyu:String
    }

    struct LuomaziData:Decodable {
        var luomazi:String
    }

    struct FanyiData:Decodable {
        var fanyi:String
    }
```

![网站json数据结构](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210907205654.jpg)



#### fetch网站API获取json数据

JSON数据结构写完之后，我们写获取网络数据function

我们要获得API中的json数据

```swift
func dancigetData(){
        let urlString = "网站API的json数据请求地址"
        let url = URL(string: urlString)
        let i = Int.random(in: 0...10)
        
        URLSession.shared.dataTask(with: url!){data, _, error in
            DispatchQueue.main.async {
                var success = false
                
                if let dancidata = data {
                    if let decodeddanci = try?
                        JSONDecoder().decode(DanciData.self, from: dancidata){
                        self.danciData.language = decodeddanci.languageData.language
                        self.danciData.riyu = decodeddanci.riyuData[i].riyu
                        self.danciData.luomazi = decodeddanci.luomaziData[i].luomazi
                        self.danciData.fanyi = decodeddanci.fanyiData[i].fanyi
                        
                        self.pandatastore.panriyu = self.danciData.riyu
                        self.pandatastore.panluomazi = self.danciData.luomazi
                        self.pandatastore.panfanyi = self.danciData.fanyi

                        
                        success = true
                        NSLog("\(success)")
                    }
                }
                if !success {
                    NSLog("Webdata: no data found")
                }
            }
        }.resume()
    }
```



我们获取到了网站JSON数据，

赋值给danciData.language, danciData.riyu, danciData,fanyi这3个全局变量，便于DanciView引用

同时，

将相应的数据赋值给了pandatastore.panriyu, pandatastore.panluomazi, pandatastore.panfanyi这3个用户默认设置变量，我们通过这3个用户默认设置变量将数据传入Complication，这样就能显示并同步更新到表盘上。

新建一个swift文件：DataStore.swift文件

这里我们建立DataStore类和PanDataStore类，里面定义一些我们要用到的全局变量和用户默认设置变量

```swift
import Foundation

class DataStore: ObservableObject{
    @Published var language: String = ""
    @Published var riyu: String = ""
    @Published var luomazi: String = ""
    @Published var fanyi: String = ""
}

private let PanriyuKey = "Panriyu"
private let PanluomaziKey = "Panluomazi"
private let PanfanyiKey = "Panfanyi"

class PanDataStore {
    let defaults = UserDefaults.standard
    
    var panriyu:String? {
        get { return defaults.object(forKey: PanriyuKey) as? String}
        set { defaults.set(newValue, forKey: PanriyuKey) }
    }
    
    var panluomazi:String? {
        get { return defaults.object(forKey: PanluomaziKey) as? String}
        set { defaults.set(newValue, forKey: PanluomaziKey) }
    }
    
    var panfanyi:String? {
        get { return defaults.object(forKey: PanfanyiKey) as? String}
        set { defaults.set(newValue, forKey: PanfanyiKey) }
    }
}

```

我们在DanciView的View下里面建立两个实例，类型是分别是DataStore和PanDataStore()

```swift
@EnvironmentObject var danciData:DataStore
let pandatastore = PanDataStore()
```

danciData的作用是获取环境变量可以使得程序各处的此类变量同时更新变化

Pandatastore的作用是设置用户默认变量，用于传输数据进入Complication



在DanciView里，我们就可以通过danciData来引用内容

danciData.riyu是获取的日语平假名

danciData.luomazi是获取的日语罗马字

danciData.fanyi是获取的中文翻译

Button内的self.dancigetData()函数用于重新fetch网络API的json数据进行内容刷新操作

```swift
Spacer()
Text("\(danciData.riyu)")
Spacer()
Text("\(danciData.luomazi)")
Spacer()
Text("\(danciData.fanyi)")
Button(""){
    self.dancigetData()
}
```



在ContentView中引用上面这个DanciView时候，还需要注意，

我们要首先定义```@ObservedObject var danciData = DataStore()```

并且使用.environmentObject(danciData)来引用，这样才能做到随时更新内容并且不产生错误

```swift
import SwiftUI

struct ContentView: View {
    
    @ObservedObject var danciData = DataStore()
    
    var body: some View {
        VStack(alignment: .leading){
            Text("\(danciData.language)")
            List {
                DanciView()
            }
        }.environmentObject(danciData)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

```



#### Complication复杂功能

显示界面非常简单，

第一行是日语平假名

第二行是日语罗马字

第三行是中文翻译。

![实机测试](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210907221613.JPG)

(A) complication复杂功能的模板内容设置

在complicationController.swift下

```swift
func getLocalizableSampleTemplate(for complication: CLKComplication, withHandler handler: @escaping (CLKComplicationTemplate?) -> Void) {
        // This method will be called once per supported complication, and the results will be cached
        switch complication.family {
            
            case .graphicRectangular:
            let template = CLKComplicationTemplateGraphicRectangularStandardBody()
            template.headerTextProvider = CLKSimpleTextProvider(text:"我是标题")
            template.body1TextProvider = CLKSimpleTextProvider(text:"我是第一行")
            template.body2TextProvider = CLKSimpleTextProvider(text:"我是第二行")
            handler(template)
            
        default:
            let template = CLKComplicationTemplateGraphicRectangularStandardBody()
            template.headerTextProvider = CLKSimpleTextProvider(text:"我是标题")
            template.body1TextProvider = CLKSimpleTextProvider(text:"我是第一行")
            template.body2TextProvider = CLKSimpleTextProvider(text:"我是第二行")
            handler(template)
            
           }
    }
```

以上为当我们在表盘设置页面选择Complication的相应程序时候默认显示的预览界面内容

![设置预览界面显示内容](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210907221806.JPG)

getLocalizableSampleTemplate就是相应的模板，我们选择graphicRectangular这个family成员，template选择GraphicRectangularStandardBody这个，具体的显示布局，苹果开发官网有详细的展示，这个比较适合我的程序。

headerTextProvider, body1TextProvider和body2TextProvider是这个template所强制要求的，我们填入内容即可，text也要用它规定的CLKSimpleTextProvider来进行赋值。

> 这里有个问题，default里面我填入其他的内容，总是会出错，因为我只用这一个特定的template，所以我就将默认的也填成同样的template内容，Xcode只是警告，但是后面运行一切正常。



(B) Complication表盘界面初始化

```swift
func getCurrentTimelineEntry(for complication: CLKComplication, withHandler handler: @escaping (CLKComplicationTimelineEntry?) -> Void) {
        // Call the handler with the current timeline entry
        let entry: CLKComplicationTimelineEntry
        
        let pan = PanDataStore()
        
        switch complication.family {
            
            case .graphicRectangular:
            let template = CLKComplicationTemplateGraphicRectangularStandardBody()
            template.headerTextProvider = CLKSimpleTextProvider(text:"\(pan.panriyu ?? "日语未刷新")")
            template.body1TextProvider = CLKSimpleTextProvider(text:"\(pan.panluomazi ?? "罗马字未刷新")")
            template.body2TextProvider = CLKSimpleTextProvider(text:"\(pan.panfanyi ?? "翻译未刷新")")
            entry = CLKComplicationTimelineEntry(date: Date(), complicationTemplate: template)
            
           default:
               preconditionFailure("Complication family not supported")
           }
           handler(entry)
    }
```

getCurrentTimelineEntry是程序在表盘的显示内容，我们通过PanDataStore()这个类的实例pan来传入数据，

pan.panriyu是日语平假名

pan.panluomazi是日语罗马字

pan.panfanyi是中文翻译

具体的内容引用赋值方式可以参照上面```fetch网站API获取json数据```这一章的内容

> ??后面的是程序没有数据传入时候默认的显示内容



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

在applicationWillResignActive这个function下，我们执行reloadActiveComplication这个function

```swift
self.reloadActiveComplications()
```

这样当我们按数码表冠回到表盘的时候，系统会重新刷新所有Complication

此时程序内的数据会显示刷新到表盘上。

显示效果如图

![applewatch演示](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo/img/20210907222032.gif)

(D) Complication定时刷新

为了让Complication定时刷新，我们要先建立时间函数

这个函数输出每个小时的第15分钟，假如现在的时间和每小时的第15分钟很接近小于5分钟，那么我们输出下一个小时的第15分钟

```swift
func nextReloadTime(after date: Date) -> Date {
        let calendar = Calendar(identifier: .gregorian)
        let targetMinutes = DateComponents(minute: 15)
     
        var nextReloadTime = calendar.nextDate (
            after: date,
            matching: targetMinutes,
            matchingPolicy: .nextTime
        )!
     
        // 如果和当前时间间隔小于 5 分钟，那么跳过，尝试下一个小时
        if nextReloadTime.timeIntervalSince (date) < 5 * 60 {
            nextReloadTime.addTimeInterval (3600)
        }
     
        return nextReloadTime
    }
```

这个函数设定下一次刷新的时间

```swift
func scheduleNextReload() {
        let targetDate = nextReloadTime (after: Date())
     
        NSLog("ExtensionDelegate: scheduling next update at %@", "\(targetDate)")
     
        WKExtension.shared ().scheduleBackgroundRefresh (
            withPreferredDate: targetDate,
            userInfo: nil,
            scheduledCompletion: { _ in }
        )
    }
```

在后台刷新函数```func handle(_ backgroundTasks: Set<WKRefreshBackgroundTask>)```下面的backgroundTasks下，我们运行

```swift
scheduleNextReload ()
DanciGetData().dancigetData()
self.reloadActiveComplications ()
NSLog("后台刷新重载")
```

当到达设定的后台运行时刻时运行上述代码，

```swift
重新设定下一次重载的时刻

运行单词获取函数dancigetData()获取网站API的JSON数据更新

重载所有Complication

控制台输出”后台刷新重载“
```

##### apple watch SE 实机测试一般会在每小时的15分到30分之间进行后台刷新并更新表盘内容



以上，我是[@PhDLuffy](https://www.zhihu.com/people/PhDLuffy)，我们都有美好的未来。

#### 感谢大家点赞、评论、分享、喜欢、收藏

#### 赞赏~

![](https://gitee.com/PhDLuffy/PicGo/raw/master/img/20200907163759.gif)

![](https://cdn.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/20210504120405.jpg)

