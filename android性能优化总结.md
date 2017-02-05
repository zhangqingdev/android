# android性能优化

### 内存检查工具
    a、Memory Monitor、Heap Viewer、Allocation Tracker 、TraceView、MAT（需要hprof文件）、Systrace、Heap Snapshot

### 布局检查工具
    a、Hierarchy Viewer

### UI重绘检查
    a、系统自带功能UI渲染检测功能
    1、重绘检查
      Debug GPU Overdraw
      作用：用来检测UI的重绘次数，开发者可以用来优化UI的性能。
      检测UI性能的利器，对于开发者做UI优化的帮助挺大的。因为大量的重绘容易让app造成卡顿或者直接导致丢帧的现象。开发者熟悉View的绘制原理可以结合对一些布局或者自定义控件做相应的优化。诸如：在ListView或GridView里面的item使用layout_weight设置就会造成多余重绘。
    2、UI绘制帧的速率和耗时
      Profile GPU Rendering
      作用：用来检测UI绘制帧的速率和耗时，同样开发者可以用来优化UI的性能。
      使用心得：跟Debug GPU Overdraw功能类似，但它反应的是UI绘制帧的速率，同样可以用来检测自己的app是否丢帧或者绘制过度


### 耗电量检查工具

### 1.Battery Historian
     a.环境安装
      1、python go  android 5.0+ 需要root adb bugreport>bugreport.txt
     b.浏览器打开battery historian界面 http://localhost:9999/
     c、上传bugreport.txt
     d、电量优化，根据Battery Historian的电量提示信息, 消耗电量包含 
       唤醒锁\SyncManager同步管理器\音视频\流量.
     优化方式: 
     (1) 检查全部唤醒锁, 是否存在冗余或者无用的位置. 
     (2) 集中相关的数据请求, 统一发送; 精简数据, 减少无用数据的传输. 
     (3) 分析和统计等非重要操作, 可以在电量充足或连接WIFI时进行, 参考JobScheduler. 
     (4) 精简冗余的服务(Service), 避免长时间执行耗电操作. 
     (5) 注意定位信息的获取, 使用后及时关闭.
### 2、android 6.0开放了电量消耗相关的API


参考链接：

http://blog.csdn.net/column/details/itfootballprefermanc.html

http://www.cnblogs.com/yezhennan/archive/2016/04.html


# android插件话

 ### 1.mutltidex  
     mutltidex 基本原理
     multiDex的基本原理是把通过DexFile来加载second dex，并存放在BaseDexClassLoader的DexPathList中。  
     存在的问题
     a.如果second.dex过大的话在应用启动的时候可能会引起anr
     b.采用multidex方案的应用需要申请一个很大的内存，在运行时可能导致程序崩溃，主要ß是由于android系统Dalvik linearAlloc的bug导致的,
       在android 4.0已经增加了linearAlloc的内存大小，但是依然有崩溃的可能(4.0以下的机型基本不用考虑了，目前4.0以下的用户量在1%-2%左右而且后续会越来越少)
       在android 5.0版本没有这个问题  因为android 5.0的系统虚拟机由dalvik切换成art虚拟机
       在ART下MultiDex是不存在这个问题的，这主要是因为ART下采用Ahead-of-time (AOT) compilation技术，系统在APK的安装过程中会使用自带的dex2oat工具对APK中可用的DEX文件进行编译并生成一个可在本地机器上运行的文件，这样能提高应用的启动速度
    采用mutltidex分包方案遇到的问题 提出解决方案
     a.加载dex放到一个异步线程中去做，这样可以提高冷启动的速度，同时还可以减少冷启动时的anr
     b.对于linearAlloc的限制，人工对DEX的拆分进行干预，使每个DEX的大小在一定的合理范围内，这样就减少触发Dalvik linearAlloc的缺陷和限制

### 2.根据业务场景拆分出各个业务线，各业务线独立dex或apk
    这种插件话的方案主要需要解决一下几个问题：
    1.dex如何动态加载
    2.插件中资源使用问题以及资源id冲突问题
    3.组件生命周期如何处理
    4.公共依赖
    
    a.插件话方案一：（这种方案用来开发简单的sdk基本可以满足使用场景，这种方案是插件话的比较早的一直方案）
      1.使用代理ProxyActivity  ProxyService完成组件的生命周期
      2.通过反射addAssetPath解决R资源的访问，资源加载到AssetManager，通过AssetManager创建Resources对象
      3. DexClassLoader
    b.插件话方案二：
      1.宿主添加占坑的Activity 占坑的Service
      2.Hook AMS PMS

  参考链接：
          http://blog.csdn.net/huangkun125/

          http://www.cnblogs.com/twlqx/p/4717035.html

          https://github.com/Qihoo360/DroidPlugin

          https://github.com/wequick/Small

          https://github.com/bunnyblue/ACDD

          https://github.com/CtripMobile/DynamicAPK

      
# android加固

### 1.资源保护
    a.mainfest.xml转换axml保护
    b.xml文件保护

### 2.代码保护
   