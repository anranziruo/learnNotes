#### 单元测试和benchmark

#### 单元测试
* 单元测试文件名必须以 _test 结尾
* 每个测试文件必须导入 testing 包
* 功能测试函数必须以 Test 开头
* 测试函数的签名必须接收一个指向testing.T类型的指针，并且不能返回任何值

#### 普通测试如下
示例代码如下:
```
func Add(a,b int) int{
	return a+b
}
```
测试代码如下:
```
func TestAdd(t *testing.T) {
	sum := Add(1,2)
	if sum == 3 {
		t.Log("the result is ok")
	} else {
		t.Fatal("the result is wrong")
	}
}
```
#### 表组测试
还有一种单元测试方法叫表组测试，这个和基本的单元测试非常相似，只不过它是有好几个不同的输入以及输出组成的一组单元测试。
表组测试我们是类似于这种方式，示例代码如下：
```
func TestAdd(t *testing.T) {
	type args struct {
		x int32
		y int32
	}
	tests := []struct {
		name string
		args args
		want int32
	}{
		{
			name: "1",
			args: args{
				x: 1,
				y: 1,
			},
			want: 2,
		},
        {
			name: "2",
			args: args{
				x: 1,
				y: 1,
			},
			want: 4,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := Add(tt.args.x, tt.args.y); got != tt.want {
				t.Errorf("Add() = %v, want %v", got, tt.want)
			}
		})
	}
}
```
运行结果如下:
```
=== RUN   TestAdd
--- FAIL: TestAdd (0.00s)
=== RUN   TestAdd/1
    --- PASS: TestAdd/1 (0.00s)
=== RUN   TestAdd/2
    --- FAIL: TestAdd/2 (0.00s)
        main_test.go:35: Add() = 2, want 4
FAIL
```
#### benchmark
* 基准测试的代码文件必须以_test.go结尾
* 基准测试的函数必须以Benchmark开头，必须是可导出的
* 基准测试函数必须接受一个指向Benchmark类型的指针作为唯一参数
* 基准测试函数不能有返回值
* b.ResetTimer是重置计时器，这样可以避免for循环之前的初始化代码的干扰
* 最后的for循环很重要，被测试的代码要放到循环里
* b.N是基准测试框架提供的，表示循环的次数，因为需要反复调用测试的代码，才可以评估性能

```
func BenchmarkSprintf(b *testing.B) {
	num := 10
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		fmt.Sprintf("%d", num)
	}
}

func BenchmarkFormat(b *testing.B) {
	num := int64(10)
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		strconv.FormatInt(num, 10)
	}
}

func BenchmarkItoa(b *testing.B) {
	num := 10
	b.ResetTimer()
	for i := 0; i < b.N; i++ {
		strconv.Itoa(num)
	}
}

```
使用命令行运行
go test -bench=. -run=none
输出的结果如下
```
goos: linux
goarch: amd64
pkg: goLirary
BenchmarkSprintf-4      10000000               102 ns/op
BenchmarkFormat-4       500000000                3.27 ns/op
BenchmarkItoa-4         500000000                3.30 ns/op
PASS
ok      goLirary        5.106s
```