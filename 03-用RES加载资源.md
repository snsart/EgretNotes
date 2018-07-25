### 资源配置文件
资源配置文件包含"resources"和"group"两部分：
##### resources
资源包含在“resources”中，游戏中所有使用到的资源都应包含在此。每一个资源拥有三个属性。
* name：对应资源的唯一ID编号
* type：资源类型
* url：当前资源的路径
##### group
组包含在“groups”中，组的概念是将不同的资源分类，当逻辑启动加载后，可以选择以组为单位进行加载。<br>
标准的资源配置如下：
```typeScript
{
    "resources":
    [
        {"name":"bgImage","type":"image","url":"assets/bg.jpg"},
        {"name":"egretIcon","type":"image","url":"assets/egret_icon.png"},
        {"name":"description","type":"json","url":"config/description.json"}
    ],
    "groups":
    [
        {"name":"preload","keys":"bgImage,egretIcon"}
    ]
}
```

### 用RES加载图片
```typeScript
RES.addEventListener(RES.ResourceEvent.GROUP_COMPLETE, this.onGroupComplete, this);
RES.loadConfig("resource/default.res.json", "resource/");//加载资源配置文件
RES.loadGroup("preload");//加载资源配置文件中preload组中的资源

private onGroupComplete()
{
     var img:egret.Bitmap = new egret.Bitmap();//创建位图
     img.texture = RES.getRes("bgImage");//为位图添加纹理
     this.addChild(img);
}
```

### RES提供的加载事件
```typeScript
* RES.addEventListener(RES.ResourceEvent.GROUP_COMPLETE, this.onResourceLoadComplete, this);//资源组加成完成监听
* RES.addEventListener(RES.ResourceEvent.GROUP_LOAD_ERROR, this.onResourceLoadError, this);//资源组加载错误监听
* RES.addEventListener(RES.ResourceEvent.GROUP_PROGRESS, this.onResourceProgress, this);//资源组加载进度监听
* RES.addEventListener(RES.ResourceEvent.ITEM_LOAD_ERROR, this.onItemLoadError, this);
* RES.loadGroup("preload");
```
这里需要注意，组加载事件回调函数里的写法，需要使用event.groupName判断下这个事件是属于哪个资源组，因为可能有多个资源组同时在加载。示例代码如下：
```typeScript
private onResourceProgress( event:RES.ResourceEvent ):void {
    if( event.groupName=="preload" ){
        this.loadingView.setProgress( event.itemsLoaded,event.itemsTotal );
    }
}
```
