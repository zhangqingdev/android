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


参考链接：http://blog.csdn.net/column/details/itfootballprefermanc.html
   