## 内容

### 基础内容

```
//移动
let action = cc.moveTo(3,650,450) //移动到坐标点
action = cc.moveBy(3,50,50) //增量移动
//旋转
action = cc.rotateTo(1,120)
//缩放
action = cc.scaleTo(1,2,0.2)
//跳跃
action = cc.jumpBy(1,100,0,100,3)
//闪烁
action = cc.blink(3,5)
//淡出
action = cc.fadeOut(3)
//淡入
action = cc.fadeIn(10)
//渐变
action = cc.fadeTo(3,100)
//颜色
action = cc.tintTo(3,100,30,100)

//设置tag
action.setTag(1)

//执行动作
this.node.runAction(action)

//结束动作
//this.node.stopAction(action)
//this.node.stopActionByTag(1) 通过tag
//this.node.stopAllActions()
//暂停动作
//this.node.pauseAllActions();
//重新动作
//this.node.resumeAllActions();
```