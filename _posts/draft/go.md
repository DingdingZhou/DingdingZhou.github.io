

由于 Go 语言是协作式的调度，不会像线程那样，在时间片用完后，由 CPU 中断任务强行将其调度走。对于 Go 语言中运行时间过长的 goroutine，Go scheduler 有一个后台线程在持续监控，一旦发现 goroutine 运行超过 10 ms，会设置 goroutine 的“抢占标志位”，之后调度器会处理。但是设置标志位的时机只有在函数“序言”部分，对于没有函数调用的就没有办法了。

func main() {
	var x int
	threads := runtime.GOMAXPROCS(0)
	for i := 0; i < threads; i++ {
		go func() {
			for { x++ }
		}()
	}
	time.Sleep(time.Second)
	fmt.Println("x =", x)
}


在四种情形下，goroutine 可能会发生调度，但也并不一定会发生，只是说 Go scheduler 有机会进行调度。

情形	说明
* 使用关键字 go	
go 创建一个新的 goroutine，Go scheduler 会考虑调度
* GC	
由于进行 GC 的 goroutine 也需要在 M 上运行，因此肯定会发生调度。当然，Go scheduler 还会做很多其他的调度，例如调度不涉及堆访问的 goroutine 来运行。GC 不管栈上的内存，只会回收堆上的内存
* 系统调用	
当 goroutine 进行系统调用时，会阻塞 M，所以它会被调度走，同时一个新的 goroutine 会被调度上来
* 内存同步访问	
atomic，mutex，channel 操作等会使 goroutine 阻塞，因此会被调度走。等条件满足后（例如其他 goroutine 解锁了）还会被调度上来继续运行



go build without further arguments builds the entire package in the current directory, including all Go and C source files.

go build main.go builds only the file main.go, as if that file is the entire Go package. (Search for “If the arguments are a list of .go files” in https://golang.org/cmd/go/#hdr-Compile_packages_and_dependencies.)

In the context of a cgo build, go build main.go would assume that all of the necessary C libraries are already compiled and installed (and presumably specified using a // #cgo LDFLAGS: -lfoo comment or similar). So if that's the difference you observed, then it sounds like everything is working as documented
