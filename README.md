# memStream
[![Build Status](https://travis-ci.org/GameXG/memStream.svg)](https://travis-ci.org/GameXG/memStream)

内存流 ，实现了 read、write、seek、close、writeTo 等方法。空间不足时自动增加空间。

## 实现的接口

```go

// 内存 Stream
// write 空间不足时会自动增加空间，但是除非关闭，否则不会释放内存。
// 由于完全使用内存存放数据，所以不要存放太多的内容。
// 预期存放很多内容请使用 ioutil.TempFile 。
// 注意：有一些方法的实现依赖于 golang 的底层实现，更换golang版本请进行单元测试后次使用。
type MemStream interface {
	io.Reader
	io.WriterTo
	io.Writer
	io.Seeker
	io.Closer
	// Len 剩余可读内容大小
	Len() int
	// 总数据大小
	Size() int64
	// 删除已读数据
	DeleteRead() error
}
func NewMemStream() MemStream {
	return &memStream{make([]byte, 0, 128), 0}
}

```