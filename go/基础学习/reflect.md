### 反射

#### 反射判断结构体
```
func assertData(dataStruct interface{},filed string)(error,bool){
    objType:= reflect.TypeOf(dataStruct)
    if objType.Elem().Kind().String()!= reflect.Struct.String(){
       return errors.New("错误的类型"),false
    }
    object := reflect.ValueOf(dataStruct)
    objElem := object.Elem()
    filedTypeValue:=objElem.FieldByName(filed)
    if !filedTypeValue.IsValid(){
        return errors.New("结构体不存在该字段"),false
    }
    switch filedTypeValue.Kind() { //根据类型判断数据值
        case reflect.Int, reflect.Int8, reflect.Int16, reflect.Int32, reflect.Int64: //int数值
        //filedTypeValue.Int() 获取Int整型数值(返回值是int64)
        case reflect.Uint, reflect.Uint8, reflect.Uint16, reflect.Uint32, reflect.Uint64://unit数值
        //filedTypeValue.Uint() 获取Uint整型数(返回值是uint64)
        case reflect.String://字符串类型
        //filedValue.String() 获取字符串的值
        case reflect.Float32, reflect.Float64://浮点型
        //filedValue.Float() 获取浮点类型(返回值是float64)
        case reflect.Bool://布尔类型
        //reflect.Bool() //返回布尔类型
    }
}
```

#### 基于反射进行数据的判断(int类型的数据的判断，其他的类似)
```
func CheckInputInt(filedValue reflect.Value, filed string, checkVal ...int64) error {
	switch filedValue.Kind() {
	case reflect.Int, reflect.Int8, reflect.Int16, reflect.Int32, reflect.Int64:
		for _, val := range checkVal {
			if filedValue.Int() == val {
				errMsg := fmt.Sprintf("%d", val)
				if errMsg == "0" {
					errMsg = "空"
				}
				return errors.New(fmt.Sprintf("输入%s字段值不能为%s", filed, errMsg))
			}
		}
	default:
		return errors.New(fmt.Sprintf("输入%s字段类型错误", filed))
	}
	return nil
}
//布尔类型是个例外
func checkInputBool(filedValue reflect.Value, filed string, checkVal bool) error {
	switch filedValue.Kind() {
	case reflect.Bool:
		if filedValue.Bool() {
			return errors.New(fmt.Sprintf("输入%s字段值不能为%v", filed, checkVal))
		}
	default:
		return errors.New(fmt.Sprintf("输入%s字段类型错误", filed))
	}
	return nil
}
```
##### 基于反射通过指针映射值
```
package main

import (
   "fmt"
   "reflect"
)

func main(){
   var pac ParamsC
   run(&pac)
   fmt.Println(pac)
}

type ParamsB struct {
   ReflectValue         reflect.Value
}

type ParamsC int


func run(paramsA interface{}){
   var pab = new(ParamsB)
   if paramsA!=nil{
      pab.ReflectValue = reflect.ValueOf(paramsA)
      for pab.ReflectValue.Kind() == reflect.Ptr {
         pab.ReflectValue = pab.ReflectValue.Elem()
      }
   }
   pab.ReflectValue.SetInt(123) //设置值
}
```

#### 通过反射给结构体映射值
```
package main

import (
   "fmt"
   "reflect"
)

func main(){
   var pac ParamsC
   run(&pac)
   fmt.Println(pac)
}

type ParamsB struct {
   ReflectValue         reflect.Value
}

type ParamsC struct {
   Name string
   Age int32
}


func run(paramsA interface{}){
   var pab = new(ParamsB)
   if paramsA!=nil{
      pab.ReflectValue = reflect.ValueOf(paramsA)
      for pab.ReflectValue.Kind() == reflect.Ptr {
         pab.ReflectValue = pab.ReflectValue.Elem()
      }
      valNumFields := pab.ReflectValue.NumField()
      for i := 0; i < valNumFields; i++ {
         filed:=pab.ReflectValue.Field(i)
         switch filed.Type().Kind() {
         case reflect.Int,reflect.Int32,reflect.Int64:
            pab.ReflectValue.Field(i).SetInt(1111)
         case reflect.String:
            pab.ReflectValue.Field(i).SetString("你好")
         }
         fmt.Println(pab.ReflectValue.Type().Field(i).Name)
         fmt.Println(filed.Type().String())
      }
   }
}
```