## 整体框架
thrift建立在Socket之上，自Socket以上整体分为三层：1.生成的客户端、服务器代码所在的应用层；2.协议层；3.传输层；


### 客户端
1. 一个客户端结构如下：
`
type EchoClient struct {
	Transport       thrift.TTransport
	ProtocolFactory thrift.TProtocolFactory
	InputProtocol   thrift.TProtocol
	OutputProtocol  thrift.TProtocol
	SeqId           int32
}
`
* 第一项表示一个传输层的链接
* 第二项用于获取具体协议的工厂
* 第三、第四项代表传输协议，通常输入协议和输出协议总是一致的;如果没有提供该值，会从工厂获取；
* 第五项是一个递增的序列号，每调用一次远程服务该值加一，该值会传输到服务端，服务端会原样返回，用作校验.


2.一个服务器结构如下：
`
type TSimpleServer struct {
	closed int32
	wg     sync.WaitGroup
	mu     sync.Mutex

	processorFactory       TProcessorFactory
	serverTransport        TServerTransport
	inputTransportFactory  TTransportFactory
	outputTransportFactory TTransportFactory
	inputProtocolFactory   TProtocolFactory
	outputProtocolFactory  TProtocolFactory
}
`
* 前三项用于服务退出时等待所有已连接请求执行完毕（注意，是链接的某次请求执行完毕）
* 第四项是生成代码中的处理请求的工厂类
* 剩下四项分别为传输层和协议层


### 协议层
协议层定义了一组接口规范，用于明确对thrift服务和数据类型的传输。thrift的服务协议必须实现该接口规范。
协议层共分为两大类，文本协议(json格式)和二进制协议，实际使用以二进制协议居多。二进制协议又分为两种，binary protocol和 compact protocol.

`
type TProtocol interface {
	WriteMessageBegin(name string, typeId TMessageType, seqid int32) error
	WriteMessageEnd() error
	WriteStructBegin(name string) error
	WriteStructEnd() error
	WriteFieldBegin(name string, typeId TType, id int16) error
	WriteFieldEnd() error
	WriteFieldStop() error
	WriteMapBegin(keyType TType, valueType TType, size int) error
	WriteMapEnd() error
	WriteListBegin(elemType TType, size int) error
	WriteListEnd() error
	WriteSetBegin(elemType TType, size int) error
	WriteSetEnd() error
	WriteBool(value bool) error
	WriteByte(value int8) error
	WriteI16(value int16) error
	WriteI32(value int32) error
	WriteI64(value int64) error
	WriteDouble(value float64) error
	WriteString(value string) error
	WriteBinary(value []byte) error

	ReadMessageBegin() (name string, typeId TMessageType, seqid int32, err error)
	ReadMessageEnd() error
	ReadStructBegin() (name string, err error)
	ReadStructEnd() error
	ReadFieldBegin() (name string, typeId TType, id int16, err error)
	ReadFieldEnd() error
	ReadMapBegin() (keyType TType, valueType TType, size int, err error)
	ReadMapEnd() error
	ReadListBegin() (elemType TType, size int, err error)
	ReadListEnd() error
	ReadSetBegin() (elemType TType, size int, err error)
	ReadSetEnd() error
	ReadBool() (value bool, err error)
	ReadByte() (value int8, err error)
	ReadI16() (value int16, err error)
	ReadI32() (value int32, err error)
	ReadI64() (value int64, err error)
	ReadDouble() (value float64, err error)
	ReadString() (value string, err error)
	ReadBinary() (value []byte, err error)

	Skip(fieldType TType) (err error)
	Flush(ctx context.Context) (err error)

	Transport() TTransport
}
`


### 传输层
thrift在golang的socket基础上添加了缓冲、分帧、压缩等功能，形成了buffered transport,framed transport,zlib transport.golang中的socket是对原生socket和epoll的封装(ubuntu16),所以thrift的传输层实现看起来异常简单，看起来像没有异步传输，线程池，reactor模式等功能一样，实则不然。





2. 服务调用的预处理
* 服务调用时的传入参数会被组装成结构体,如下示例的一个参数封装：
`
EchoRes echo(1: EchoReq req,2: string id); 
=>
type EchoArgs struct {
	Req *EchoReq `thrift:"req,1"`
	Id  string   `thrift:"id,2"`
}
`
封装后的结构体，会包含一个thrift的tag，tag的内容是需要传输的参数名称和参数id，但并不会动态解析其内容
对于传输结构体的每一个域，都会先传输参数名称和参数id，在向后兼容时会用到,但是golang生成的代码里检测只用了参数id，并没有用参数名称

3. 一次调用的发送过程
* 传输服务名称、请求类型（请求服务类型）、序列号
* 传输服务参数
* 传输服务结束标志
* 刷新缓冲区（前几步的传输可能只是写入到缓冲区,具体由传输层控制）


4. 一次调用的接收过程
* 读取返回类型，返回序列号;如果返回类型为异常，读取异常，异常的读取方式与结构体一致；判断返回序列号是否与发送的一致
* 读取返回结果
* 读取结束标志(内部实现可能什么也没做,具体由协议层控制)

5. 向后兼容
* 向后兼容与读取方法中的skip（）方法有关，主要是在协议层进行的处理
	






## 协议层
* binary protocol 二进制协议
* compact protocol 紧凑型二进制协议
* json protocol 文本json格式协议


## 传输层
* buffered transport 带缓冲的传输层



