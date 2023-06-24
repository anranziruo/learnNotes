### Functional Options
functional Options Pattern（函数式选项模式）
Option模式为golang的开发者提供了将一个函数的参数设置为可选的功能，也就是说我们可以选择参数中的某几个，并且可以按任意顺序传入参数

优点:
1.支持传递多个参数，并且在参数个数、类型发生变化时保持兼容性
2.任意顺序传递参数
3.支持默认值
4.方便拓展

以构建一个server为示例
```
type Server struct {
    Addr     string
    Port     int
    Protocol string
    Timeout  time.Duration
    MaxConns int
    TLS      *tls.Config
}
```
以option构造的示例代码如下:
```
package main

import (
	"crypto/tls"
	"fmt"
	"time"
)

type Server struct {
	Addr     string
	Port     int
	Protocol string
	Timeout  time.Duration
	MaxConns int
	TLS      *tls.Config
}

type Option func(*Server)

func NewServer(addr string, port int, options ...Option) (*Server, error) {
	s := &Server{
		Addr:     addr,
		Port:     port,
		Protocol: "tcp",            // 默认参数
		Timeout:  30 * time.Second, // 默认参数
		MaxConns: 1000,             // 默认参数
	}

	for _, option := range options {
		option(s)
	}

	return s, nil
}

func WithProtocol(protocol string) Option {
	return func(s *Server) {
		s.Protocol = protocol
	}
}

func WithTimeout(timeout time.Duration) Option {
	return func(s *Server) {
		s.Timeout = timeout
	}
}

func WithMaxConns(maxConns int) Option {
	return func(s *Server) {
		s.MaxConns = maxConns
	}
}

func main() {

	// 默认参数使用
	s1, err := NewServer("localhost", 1024)
	if err != nil {
		panic(err)
	}
	fmt.Println(s1)
	s2, err := NewServer("localhost", 1024, WithProtocol("udp"))
	if err != nil {
		panic(err)
	}
	fmt.Println(s2)
}
```

