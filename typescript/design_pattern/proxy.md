##

### 内容
```
interface Req {
    Request():void;
}

class IoReq implements Req{
    Request(): void {
        console.log("IoReq")
    }
}

class ProxyReq implements Req{

    private ioReq:IoReq;

    constructor(ioReqObj:IoReq){
        this.ioReq = ioReqObj
    }

    Request(): void {
        this.ioReq.Request()
    }
}

const ioReqObj = new IoReq()

const reqObj = new ProxyReq(ioReqObj)
reqObj.Request()
```