# Go语言
## Go语言经典语法问题
1、函数执行时，如果由于panic导致了异常，则延迟函数不会执行。这一说法是否正确？
这个说法是不正确的。当一个 Go 函数由于 panic 异常而退出时，所有已经注册的延迟函数(defer)都会被执行，然后 panic 异常会继续向外传递。 也就是说，在 Go 中，延迟函数(defer)的执行是不受 panic异常的影响的。

在 Go 中，defer 的实现原理是通过栈的方式来实现的。

当一个 defer 语句被执行时，它所在的函数的信息会被保存在一个栈中，同时 defer 语句中的函数也会被保存在另外一个栈中。如果函数正常返回，那么 defer 栈中的函数会被按照后进先出（LIFO）的顺序执行。如果函数由于 panic 异常而退出，那么 defer 栈中的函数同样会被执行，也按照 LIFO 的顺序依次执行。

defer 的使用场景主要包括资源释放、异常处理、代码简化等方面。当我们需要在函数返回前执行一些操作，或者在程序出现异常时进行一些清理工作，或者需要将一些函数调用作为一个整体进行执行时，都可以使用 defer 来满足这些需求。

2、对于以下代码，描述正确的是：
```Go
func myTest1() {
	var wg sync.WaitGroup
	intSlice := []int{1, 2, 3, 4, 5}
	wg.Add(len(intSlice))
	ans1, ans2 := 0, 0
	for _, v := range intSlice {
		vv := v
		go func() {
			fmt.Printf("v = %v,vv=%v\n", v, vv)
			defer wg.Done()
			ans1 += v
			ans2 += vv
		}()
	}
	wg.Wait()
	fmt.Printf("ans1:%v,ans2:%v", ans1, ans2)
}
```
ans1和ans2都不一定为15。这段代码的执行结果是不确定的，因为多个 goroutine 并发执行，所以 ans1 和 ans2 的在该代码中，ans1 和 ans2 的初始值都为 0，多个 goroutine 并发执行 ans1 += v 和 ans2 += vv，会导致数据竞争，因此无法确定它们的最终值。虽然在该代码中，vv 变量已经通过值传递传递到了匿名函数里面，但是在 goroutine 开始执行之前循环可能已经继续执行了，此时 vv 变量的值可能会被修改。这就会导致 ans2 的值无法确定，因为 ans2 受制于 vv 的值，而 vv 的值在多个 goroutine 并发执行时可能不是我们预期的。因此，对于这种情况，我们需要使用 sync.Mutex 进行加锁来保证多个 goroutine 不会同时访问和修改变量的值。

3、golang是一门面向对象的语言吗？
Golang 是一门面向对象的编程语言，但是它比传统的面向对象语言更加强调组合而非继承来实现代码复用。在 Golang 中，类（class）的概念被替换为结构体（struct）和方法（method），它们可以实现封装、继承和多态等面向对象的特性。同时，Golang 也提供了接口（interface）来实现多态性，但是接口的实现是由具体的类型来实现的，而不是基于继承的子类继承父类的方式。

因此，虽然 Golang 不像 Java 或 C# 那样强调面向对象编程，但是它仍然是一门支持面向对象编程的语言。

## Go语言经典面试问题
1、golang中切片容量增长
在golang1.18版本后，扩容策略变为：当原slice容量(oldcap)小于256的时候，新slice(newcap)容量为原来的2倍；原slice容量超过256，新slice容量newcap = oldcap+(oldcap+3*256)/4

2、下面的例子显示了，在函数中改变一个map在调用方的map也会跟着改变
在函数中修改slice的值调用方的slice也会改变，但append则不一定，因为可能发生扩容
```Go
import "fmt"

func main() {
	myMap := map[string]interface{}{}
	myMap["abc"] = 1
	fmt.Println(myMap)
	changeMap(myMap)
	fmt.Println(myMap)

	intSlice := []int{1, 2, 3, 4}
	fmt.Println(intSlice)
	changeSlice(intSlice)
	fmt.Println(intSlice)
	appendSlice(intSlice)
	fmt.Println(intSlice)
}

//在函数中改变一个map在主函数中也会改变
func changeMap(mtc map[string]interface{}) {
	mtc["efg"] = 2
}

func changeSlice(intSlice []int) {
	for i := range intSlice {
		intSlice[i] = 4
	}
}

func appendSlice(intSlice []int) {
	intSlice = append(intSlice, 0)
}
```

3、Golang的channel是如何实现并发安全的
Go 语言中的 channel 通过使用并发原语实现并发安全。在底层，channel 是通过使用同步原语（mutexes、condition variables）和原子操作（atomic operations）来实现的，并且遵循 happens-before 原则。

具体来说，channel 有两个基本操作：发送和接收。在发送过程中，channel 首先会检查是否有其他 goroutine 正在接收 channel 中的值，如果有，则将值直接传递给接收方，并激活接收方的 goroutine。如果没有，则将值放入一个队列中，并等待其他 goroutine 执行接收操作。在接收过程中，channel 会检查是否有其他 goroutine 正在发送值，如果有，则直接获取该值并返回。如果没有，则从队列中取出一个值，并将其传递给发送方，然后激活发送方的 goroutine。

这些操作都是原子的，因此 channel 可以安全地在多个 goroutine 之间共享。此外，Go 语言中的 channel 还具有先进先出的特性，保证了多个 goroutine 在使用同一个 channel 时不会出现竞争条件的问题。

需要注意的是，虽然 channel 是并发安全的，但是如果使用不当，仍然可能会出现一些问题，如死锁、阻塞等。因此，在使用 channel 时应该注意它们的容量、发送和接收的时机，以及它们的使用方式。