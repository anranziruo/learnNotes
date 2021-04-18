### zap日志
Zap是非常快的、结构化的，分日志级别的Go日志库

#### 安装方法

```
go get -u go.uber.org/zap
```

#### 使用指南
```
1.通过zap.NewProduction()/zap.NewDevelopment()或者zap.Example()创建一个Logger
2.production logger默认记录调用函数信息、日期和时间等
3.支持各个级别的日志输出

示例如下：
package zap_log

import (
	"go.uber.org/zap"
)

var zapLogger *zap.Logger

func init(){
	zapLogger,_= zap.NewProduction()
}

func Print(){
	Print2()
	zapLogger.Info("print")
}

func Print2(){
	zapLogger.Debug("print2")
	PrintE()
}

func PrintE(){
	zapLogger.Error("big error crash")
}
```
#### sugared logger和logger的区别
在性能很好但不是很关键的上下文中，使用SugaredLogger。它比其他结构化日志记录包快4-10倍，并且支持结构化和printf风格的日志记录。
在每一微秒和每一次内存分配都很重要的上下文中，使用Logger。它甚至比SugaredLogger更快，内存分配次数也更少，但它只支持强类型的结构化日志记录

示例如下：
```
var zapLogger *zap.SugaredLogger

func init(){
	logger,_:= zap.NewProduction()
	zapLogger=logger.Sugar()
}

func Print(){
	Print2()
	zapLogger.Infof("print")
}

func Print2(){
	zapLogger.Debugf("print2")
	PrintE()
}

func PrintE(){
	zapLogger.Errorf("big error crash")
}
```

#### 日志写入文件
```
var zapLoggerf *zap.Logger

func init(){
	file, _ := os.Create("./test.log") //生成文件句柄
	writeSync:=zapcore.AddSync(file)

    //encode的配置
	encoderConfig := zap.NewProductionEncoderConfig()
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder //iso时间
	encoderConfig.EncodeLevel = zapcore.CapitalLevelEncoder
	encoder:=zapcore.NewJSONEncoder(encoderConfig)
	
	core:=zapcore.NewCore(encoder,writeSync,zapcore.DebugLevel)
	zapLoggerf= zap.New(core) 
}
```

#### 日志写入kafka
```
1.首先kafka实现 io.write的interface方法

type kafkaProducer struct {
	topic string
	syncProducer sarama.SyncProducer
}

func (kp *kafkaProducer) Write(p []byte) (n int, err error)  {
	_,_, err = kp.syncProducer.SendMessage(&sarama.ProducerMessage{
		Topic: kp.topic,
		Value: sarama.ByteEncoder(p),
	})
	if err != nil {
		log.Println(err)
		return 0,err
	}
	//fmt.Println("send msgs:",string(p))
	return len(p),nil
}
2.转化成通用的配置项
kp,err := newKafkaProducer(producerTopic,"localhost:9092")
	if err != nil {
		log.Fatal(err)
	}
kws := zapcore.AddSync(kp)
kafkaPriority := zap.LevelEnablerFunc(func(lev zapcore.Level) bool{
		return lev >= zap.DebugLevel
})
kafkaCore := zapcore.NewCore(zapcore.NewJSONEncoder(prodEncoder),kws,kafkaPriority)
```