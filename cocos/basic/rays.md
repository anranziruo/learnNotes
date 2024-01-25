## 内容

### 初始化物理系统
首先物理系统的初始化必须要在onload方法里面做，示例如下：
```
onLoad () {
    cc.director.getPhysicsManager().enabled = true
}
```

### 设置射线方向
```
directionY:cc.Vec2 = cc.v2(0,1)  //向上
directionDY:cc.Vec2=cc.v2(0,-1)  //向下
directionx:cc.Vec2 = cc.v2(1,0)  //向右
directionDx:cc.Vec2=cc.v2(-1,0)  //向左
```

### 属性检查器给被碰撞物体添加物理碰撞属性
```
物理组件->collider->Box
```

### 获取碰撞结果
```
switch(this.flag%5){
case 0:
    this.moveDirection = this.directionY
    break
case 1:
    this.moveDirection = this.directionDx
    break
case 2:
    this.moveDirection = this.directionx
    break
case 3:
    this.moveDirection = this.directionDY
    break
case 4:
    this.moveDirection = this.directionx
    break
}
this.node.x += this.moveDirection.x *100 *dt
this.node.y += this.moveDirection.y *100 *dt

//执行射线结果
let results = cc.director.getPhysicsManager().rayCast(cc.v2(this.node.position),cc.v2(this.node.x+this.moveDirection.x*50,
this.node.y+this.moveDirection.y*50),cc.RayCastType.Closest)
if(results.length >0){
    this.flag = this.flag +1
}
```