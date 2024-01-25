## 内容


### 初始化物理系统
首先物理系统的初始化必须要在onload方法里面做，示例如下：
```
onLoad () {
    cc.director.getPhysicsManager().enabled = true
}
```

### 控制速度
```
let rd = this.getComponent(cc.RigidBody)
rd.linearVelocity = cc.v2(50,0)
```

### 检测碰撞事件
```
onBeginContact(contact,self,other){
    //获取碰撞点
    let points = contact.getWorldManifold().points
    console.log(points)
    //法线
    let normal = contact.getWorldManifold().points
}
```
### senSor属性
```
会产生碰撞回调，但是不会产生碰撞效果

onBeginContact(contact,self,other){
    let points = contact.getWorldManifold().points
    console.log(points)
    let normal = contact.getWorldManifold().normal
    console.log(normal)
}
onEndContact(contact,self,other){
    console.log("结束碰撞回调")
}
```
会输出结束碰撞回调
适用场景:隐藏宝箱，隐藏副本