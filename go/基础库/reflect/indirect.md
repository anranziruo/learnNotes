#### 使用含义
```
间接返回 v 指向的值。如果 v 是一个零指针，则间接返回一个零值。如果 v 不是指针，则间接返回 v
reflect.Value是指针，则v.Elem()等效于reflect.Indirect(v)。如果不是指针，则它们不是等效的：
如果该值是接口，reflect.Indirect(v)则将返回相同的值，而v.Elem()将返回所包含的动态值。
如果该值是其他值，则将感到v.Elem()恐慌。
该reflect.Indirect助手用于需要接受特定类型或指向该类型的指针的情况。一个示例是database/sql转换例程：通过使用reflect.Indirect，它可以使用相同的代码路径来处理各种类型和指向这些类型的指针
```

#### 参考示例
```
value := reflect.ValueOf(dest)
// json.Unmarshal returns errors for these
if value.Kind() != reflect.Ptr {
    return errors.New("must pass a pointer, not a value, to StructScan destination")
}
if value.IsNil() {
    return errors.New("nil pointer passed to StructScan destination")
}
direct := reflect.Indirect(value)
```


