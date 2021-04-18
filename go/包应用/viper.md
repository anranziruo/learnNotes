### viper配置
Viper是适用于Go应用程序（包括Twelve-Factor App）的完整配置解决方案。它被设计用于在应用程序中工作，并且可以处理所有类型的配置需求和格式
支持JSON、TOML、YAML、HCL、envfile和Java properties

#### 嵌套json的使用
```
{
  "WebPort":":8094",
  "Redis":{
      "ChatRedis":{
          "Addr":"127.0.0.1:6379",
          "Password": "",
          "DB": 1,
          "PoolSize": 15
      }
  },
  "Mysql":{
    "FinanceR":{
      "Url": "root:root@tcp(localhost:3306)/finance?charset=utf8&parseTime=true&",
      "MaxOpensConns": 10,
      "MaxIdleConns": 5
    },
    "FinanceW":{
      "Url": "root:root@tcp(localhost:3306)/finance?charset=utf8&parseTime=true&",
      "MaxOpensConns": 10,
      "MaxIdleConns": 5
    }
  }
}

初始化的使用
  s.Config = viper.New() //初始化
	env := os.Getenv("admin_env")
	if env == "" {
		env = "dev"
	}
	configName := fmt.Sprintf("%s.json", env)
	s.Config.SetConfigName(configName)
	s.Config.AddConfigPath("conf") //配置文件路径
	s.Config.SetConfigType("json") //json的格式
	err := s.Config.ReadInConfig()
	if err != nil {
		log.Printf("ConfigLoader.LoadConfig err:%s", err.Error())
	}

    //获取嵌套json
    FinanceWDB, err = sql.Open("mysql", config.GetStringValue("Mysql.FinanceW.Url"))
```
#### 设置默认值(可以设置默认值)
```
package vp

import (
	"fmt"
	"github.com/spf13/viper"
)

var Vip *viper.Viper

func init(){
	InitVp()
}

func InitVp(){
	Vip = viper.New()
	Vip.SetDefault("env","dev")
}

func GetDefaultV()  {
	fmt.Println(Vip.Get("env"))
}
```
#### 动态监控值
```
var Vip *viper.Viper

func init(){
	InitVp()
}

func InitVp(){
	Vip = viper.New()
	Vip.SetConfigFile("conf/cn.yml")
	Vip.SetConfigType("yaml")
	err := Vip.ReadInConfig()
	if err!=nil{
		log.Println(err)
	}
}

func WatchV()  {
	Vip.WatchConfig()
	Vip.OnConfigChange(func(e fsnotify.Event) {
		fmt.Println(Vip.Get("hobbies"))
	}) //监控文件变化
}
```

#### 以字符串初始化
```
func InitVp(){
	Vip = viper.New()
	Vip.SetConfigType("yaml")
	var yamlExample = []byte(`
Hacker: true
name: steve
hobbies:
- skateboarding
- snowboarding
- go
clothing:
  jacket: leather
  trousers: denim
age: 35
eyes : brown
beard: true
`)

	err := Vip.ReadConfig(bytes.NewBuffer(yamlExample))
	if err!=nil{
		log.Println(err)
	}
}

func Print()  {
	fmt.Println(Vip.Get("hobbies"))
}
```