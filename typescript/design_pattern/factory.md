## 内容

### 简单工厂模式
```
class Operate {
    public Result():void{}
}

enum Operator {
    Add,
    Divide,
    Mul
}

class AddMethod extends Operate{
     public Result(): void {
        
    }
}
class DivideMethod extends Operate{
    public Result(): void {
        
    }
}
class MulMethod extends Operate{
    public Result(): void {
        
    }
}

class OperateFacatory{
    public static CreateFactory(operateType:Number):Operate{
        switch(operateType){
            case Operator.Add:
                return new AddMethod;
            case Operator.Divide:
                return new DivideMethod;
            case Operator.Mul:
                return new MulMethod;
            default:
                return null
        }
    }
}

console.log(OperateFacatory.CreateFactory(0))
console.log(OperateFacatory.CreateFactory(4))
```