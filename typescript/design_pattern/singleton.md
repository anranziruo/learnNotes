## 内容

### 单例模式
```
class Singleton{
    private static instance:Singleton = null; //静态常量

    public static Instance():Singleton{ //静态方法,只实例化一次
        if(Singleton.instance == null){
            Singleton.instance = new Singleton();
        }
        return Singleton.instance
    }
}

const s1 = Singleton.Instance();
const s2 = Singleton.Instance();
if (s1 === s2) {
    console.log('Singleton works, both variables contain the same instance.');
} else {
    console.log('Singleton failed, variables contain different instances.');
}
```