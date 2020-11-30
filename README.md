# go-exp

## Use channel to send and receive data
```
package main

import (
	"fmt"
)

func main() {
	messages := make(chan string)
	go func() { messages <- "ping" }()

	msg := <-messages
	fmt.Println(msg)
}
```

## main goroutine & function goroutine
```
package main

import (
	"fmt"
	"time"
)

func newTask() {
	i := 0
	for {
		i++
		fmt.Printf("new goroutine: i = %d\n", i)
		time.Sleep(1 * time.Second) //延时1s
	}
}

func main() {
	//创建一个 goroutine，启动另外一个任务
	go newTask()

	i := 0
	//main goroutine 循环打印
	for {
		i++
		fmt.Printf("main goroutine: i = %d\n", i)
		time.Sleep(1 * time.Second) //延时1s
	}
}
```
## Main goroutine exits first
```
package main

import (
	"fmt"
	"time"
)

func newTask() {
	i := 0
	for {
		i++
		fmt.Printf("new goroutine: i = %d\n", i)
		time.Sleep(1 * time.Second) //延时1s
	}
}

func main() {
	//创建一个 goroutine，启动另外一个任务
	go newTask()

	fmt.Println("main goroutine exit")
}
```
## use runtime.Gosched()
```
package main

import (
	"fmt"
	"runtime"
)

func main() {
	//创建一个goroutine
	go func(s string) {
		for i := 0; i < 2; i++ {
			fmt.Println(s)
		}
	}("world")

	for i := 0; i < 2; i++ {
		runtime.Gosched() //import "runtime"
		/*
		   屏蔽runtime.Gosched()运行结果如下：
		       hello
		       hello

		   没有runtime.Gosched()运行结果如下：
		       world
		       world
		       hello
		       hello
		*/
		fmt.Println("hello")
	}
}
```
## runtime.GOMAXPROCS()
```
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	//n := runtime.GOMAXPROCS(1) //打印结果：111111111111111111110000000000000000000011111...
	n := runtime.GOMAXPROCS(2) //打印结果：010101010101010101011001100101011010010100110...
	fmt.Printf("n = %d\n", n)

	for{
		go fmt.Print(0)
		fmt.Print(1)
		time.Sleep(1 * time.Second) //延时1s
	}

}
```
