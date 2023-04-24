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

4、

## Go语言经典面试问题
