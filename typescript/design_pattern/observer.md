## 内容

### 观察者模式

```
interface Observer{
    update(subject:Subject):void;
}

interface Subject{
    AddObs(ob:Observer):void;
    DelObs(ob:Observer):void;
    Notify():void;
}

class ConcreteSubject implements Subject{

    public msg:string;

    private observers :Observer[] = []
    AddObs(ob: Observer): void {
        const exist = this.observers.includes(ob)
        if(!exist){
            this.observers.push(ob)
        }
    }
    DelObs(ob: Observer): void {
        const index = this.observers.indexOf(ob)
        if(index>0){
            this.observers.splice(index, 1);
        }
    }
    Notify(): void {
        for(const ob of this.observers){
            ob.update(this)
        }
    }

    SendMsg(msg:string):void{
        this.msg = msg
        this.Notify()
    }
}

class ObserverA implements Observer{
    update(subject: Subject): void {
        console.log(subject)
    }
}

class ObserverB implements Observer{
    update(subject: Subject): void {
        console.log(subject)
    }
}

const subject = new ConcreteSubject();

const observer1 = new ObserverA();
subject.AddObs(observer1);

const observer2 = new ObserverB();
subject.AddObs(observer2);

subject.SendMsg("hello");

subject.DelObs(observer2)

subject.SendMsg("hello1");
```