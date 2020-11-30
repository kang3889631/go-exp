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
