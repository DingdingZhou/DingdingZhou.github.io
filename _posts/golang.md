## Iteration Values For Strings in "range" Clauses

level: beginner

The index value (the first value returned by the "range" operation) is the index of the first byte for the current "character" (unicode code point/rune) returned in the second value. It's not the index for the current "character" like it's done in other languages. Note that an actual character might be represented by multiple runes. Make sure to check out the "norm" package (golang.org/x/text/unicode/norm) if you need to work with characters.

The for range clauses with string variables will try to interpret the data as UTF8 text. For any byte sequences it doesn't understand it will return 0xfffd runes (aka unicode replacement characters) instead of the actual data. If you have arbitrary (non-UTF8 text) data stored in your string variables, make sure to convert them to byte slices to get all stored data as is.



## Unexported Structure Fields Are Not Encoded

level: beginner
The struct fields starting with lowercase letters will not be (json, xml, gob, etc.) encoded, so when you decode the structure you'll end up with zero values in those unexported fields.


## a simple way to signal all goroutines at once
One of the most common solutions is to use a "WaitGroup" variable. It will allow the main goroutine to wait until all worker goroutines are done. If your app has long running workers with message processing loops you'll also need a way to signal those goroutines that it's time to exit. You can send a "kill" message to each worker. Another option is to close a channel all workers are receiving from. It's a simple way to signal all goroutines at once.


## Using "nil" Channels
level: beginner
Send and receive operations on a nil channel block forver. It's a well documented behavior, but it can be a surprise for new Go developers.
fatal error: all goroutines are asleep - deadlock!


## 
channel的使用channel支持以下操作使用cap(ch)函数查询channel的容量，cap是golang的内置函数使用len(ch)函数查询channel内部的数据长度，len函数也是内置的，表面上这个函数很有意义，但实际上它很少用。使用close(ch)关闭channel，close也是内置函数。一个非空channel只能够被关闭一次，如果关闭一个已经被关闭的或者是关闭一个空channel将会引发panic。另外关闭一个只读channel是非法的，编译器直接报错。使用ch <- v发送一个值v到channel。发送值到channel可能会有多种结果，即可能成功，也可能阻塞，甚至还会引发panic，取决于当前channel在什么状态。使用 v, ok <- ch 接收一个值。第二个遍历ok是可选的，它表示channel是否已关闭。接收值只会又两种结果，要么成功要么阻塞，而永远也不会引发panic。所有的这些操作都是同步的协程安全的，不需要加任何其它同步控制。


## Closing HTTP Response Body
Most of the time when your http request fails the resp variable will be nil and the err variable will be non-nil. However, when you get a redirection failure both variables will be non-nil. This means you can still end up with a leak.

## Unmarshalling JSON Numbers into Interface Values
level: intermediate
By default, Go treats numeric values in JSON as float64 numbers when you decode/unmarshal JSON data into an interface. This means the following code will fail with a panic:



## Comparing Structs, Arrays, Slices, and Maps
level: intermediate
You can use the equality operator, ==, to compare struct variables if each structure field can be compared with the equality operator.

Go does provide a number of helper functions to compare variables that can't be compared using the comparison operators.
The most generic solution is to use the DeepEqual() function in the reflect package.


## Recovering From a Panic
level: intermediate
The recover() function can be used to catch/intercept a panic. Calling  recover() will do the trick only when it's done in a deferred function.

The call to recover() works only if it's called directly in your deferred function.


## Updating and Referencing Item Values in Slice, Array, and Map "range" Clauses

level: intermediate
The data values generated in the "range" clause are copies of the actual collection elements. They are not references to the original items. This means that updating the values will not change the original data. It also means that taking the address of the values will not give you pointers to the original data.


## "Hidden" Data in Slices
level: intermediate
When you reslice a slice, the new slice will reference the array of the original slice. If you forget about this behavior it can lead to unexpected memory usage if your application allocates large temporary slices creating new slices from them to refer to small sections of the original data.

To avoid this trap make sure to copy the data you need from the temporary slice (instead of reslicing it).

## Iteration Variables and Closures in "for" Statements
注意，for循环内不要轻易开协程

level: intermediate
This is the most common gotcha in Go. The iteration variables in for statements are reused in each iteration. This means that each closure (aka function literal) created in your for loop will reference the same variable (and they'll get that variable's value at the time those goroutines start executing).


## Deferred Function Call Argument Evaluation
level: intermediate

Arguments for a deferred function call are evaluated when the defer statement is evaluated (not when the function is actually executing). The same rules apply when you defer a method call. The structure value is also saved along with the explicit method parameters and the closed variables.

If you have pointer parameters it is possible to change the values they point to because only the pointer is saved when the defer statement is evaluated.


## Deferred Function Call Execution
level: intermediate
The deferred calls are executed at the end of the containing function (and in reverse order) and not at the end of the containing code block. It's an easy mistake to make for new Go developers confusing the deferred code execution rules with the variable scoping rules. It can become a problem if you have a long running function with a for loop that tries to defer resource cleanup calls in each iteration.



## slice
切片出现问题的原因，在于reslice


## Iteration Variables and Closures in "for" Statements
level: intermediate
This is the most common gotcha in Go. The iteration variables in for statements are reused in each iteration. This means that each closure (aka function literal) created in your for loop will reference the same variable (and they'll get that variable's value at the time those goroutines start executing).






##  Contexts
Don't add a Context member to a struct type; instead add a ctx parameter to each method on that type that needs to pass it along. The one exception is for methods whose signature must match an interface in the standard library or in a third party library.


Don't create custom Context types or use interfaces other than Context in function signatures.

## Copying
To avoid unexpected aliasing, be careful when copying a struct from another package. For example, the bytes.Buffer type contains a []byte slice and, as an optimization for small strings, a small byte array to which the slice may refer. If you copy a Buffer, the slice in the copy may alias the array in the original, causing subsequent method calls to have surprising effects.

In general, do not copy a value of type T if its methods are associated with the pointer type, *T.




## Declaring Empty Slices
When declaring an empty slice, prefer
var t []string
over
t := []string{}

The former declares a nil slice value, while the latter is non-nil but zero-length. They are functionally equivalent—their len and cap are both zero—but the nil slice is the preferred style.

Note that there are limited circumstances where a non-nil but zero-length slice is preferred, such as when encoding JSON objects (a nil slice encodes to null, while []string{} encodes to the JSON array []).


## Crypto Rand

Do not use package math/rand to generate keys, even throwaway ones. Unseeded, the generator is completely predictable. Seeded with time.Nanoseconds(), there are just a few bits of entropy. Instead, use crypto/rand's Reader, and if you need text, print to hexadecimal or base64:



## json.Marshal
匿名继承导致的json tag字段冲突，会导致无法输出任何内容



# named return
Naked returns are okay if the function is a handful of lines. Once it's a medium sized function, be explicit with your return values. Corollary: it's not worth it to name result parameters just because it enables you to use naked returns. Clarity of docs is always more important than saving a line or two in your function.

Finally, in some cases you need to name a result parameter in order to change it in a deferred closure. That is always OK.

