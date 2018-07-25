egret中提供的缓动库用法和createjs等语言的缓动用法基本一致，不过在egret的Tween中提供了缓动变化事件，可以在缓动的过程中不断的监听缓动对象变化的属性。
### 缓动变化事件
在 Tween 执行过程中，可能逻辑需要实时做一些变化。跟踪这个过程同样可以通过在 Tween.get() 的第二个参数中，加入变化事件处理函数的定义来实现。比如游戏中有猎物在做一个Tween运动过程中，猎人的枪口要实时瞄准，那么就需要在Tween的变化过程随时计算猎物位置，修正猎人枪口的角度。
```typeScript
var obj = { x:0 };
var funcChange = function():void{
    console.log( this.x );//log出缓动对象变化的坐标
}
egret.Tween.get( obj, { onChange:funcChange, onChangeObj:obj } )
    .to( {x:600}, 1000 , egret.Ease.backInOut );
```
