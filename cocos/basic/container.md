## 内容


### 基础内容
```
start () {
    //显示
    let action = cc.show()
    //隐藏
    action = cc.hide()
    //根据上一个状态切换
    action = cc.toggleVisibility()
    //翻转
    action = cc.flipX(true)
    action = cc.flipY(true)
    //回调动作
    let callAction = cc.callFunc(()=>{
        console.log("打印我了")
    })
    //队列/封装一系列操作/容器
    let fadeInAction = cc.fadeOut(1)
    let fadeOutAction = cc.fadeIn(2)
    //cc.delayTime(3) 延迟三秒
    let seq = cc.sequence(fadeInAction,fadeOutAction,cc.delayTime(3),callAction)
    //设置重复次数
    let repeat = cc.repeat(seq,4)
    //设置永远不停止
    let repeatForever = cc.repeatForever(seq)
    //并列动作同时执行
    let move = cc.moveTo(3,300,600)
    let tiny = cc.tintTo(3,100,100,29)
    let spawn = cc.spawn(move,tiny)
    this.node.runAction(seq)
}
```