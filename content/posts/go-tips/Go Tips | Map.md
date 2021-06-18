+++
author = "xblzbjs"
title = "Go Tips | Map"
date = "2021-05-25"
slug = "Go Tips Map"
series = [
	"go-tips",
]
tags = [
	"go",
]
+++


## Tip1 初始化方式

```go
func TestTestInitMap(t *testing.T) {
	// 方法一（字面量初始化）
	m1 := map[string]int{"Go": 1, "Java": 2, "Python": 3}
	m2 := map[int]int{}
	// 方法二（使用内置函数make初始化）
	m3 := make(map[int]int, 10)
	// 使用len函数打印map的长度, 不能用cap打印容量
	t.Logf("len m1=%d", len(m1))
	m2[4] = 16
	t.Logf("len m2=%d", len(m2))
	t.Logf("len m3=%d", len(m3)) 
}
```

以上两者同样是初始化一个Map，有什么不同？

- 方法二提前分配了Map的容量，减少内存分配的次数，相比方法一而言性能更好，但是可扩展性更差。

## Tip2 增删查改（Create、Delete、Read、Update）

```go
func TestMapCRUD(t *testing.T) {
	m := make(map[string]int, 10)
	m["other"] = 0
	// Create
	m["go"] = 1
	t.Logf("Before updating:%d", m["go"])
	// Update
	m["go"] = 100
	t.Logf("After updating:%d", m["go"])
	// Delete
	delete(m, "go")
	// Read
	val, exist := m["go"]
	if exist {
		t.Logf("Exist! Value is %d", val)
	} else {
		t.Log("Does not exist!")
	}
}

func TestEmptyMap(t *testing.T) {
	var m map[string]int
	m["string"] = 3	//会触发panic: assignment to entry in nil map
}
```

注意：

- 如果map为nil或者指定key不存在时，delete()不会报错，相当于空操作
- 查询、修改键时，检查键是否存在。否则查询会操作零值，修改会创建新的键值对
- **未初始化的map值为nil，向值为nil的map添加元素时会出发panic**

## Tip3 遍历输出Map的key与value

```go
func TestTravelMap(t *testing.T) {
	m1 := map[int]int{1: 1, 2: 4, 3: 9}
	for k, v := range m1 {
		t.Log(k, v)
	}
}
```



## Tip4  `new`和`make`创建Map的差异

使用new来创建map时，返回的内容是一个**指针**，这个指针指向了一个所有字段全为0的值map对象，**需要初始化**后才能使用，而使用make来创建map时，返回的内容是一个**引用**，可以**直接使用**。

### 例子

```go
func TestDiffMakeAndNew(t *testing.T) {
	t.Log("========使用new创建Map=======")
	m := new(map[string]int)
	t.Log(m, "\t", reflect.TypeOf(m))
	t.Log(&m, "\t", reflect.TypeOf(&m))
	t.Log(*m, "\t", reflect.TypeOf(*m))
	*m = map[string]int{}
	t.Log(*m, "\t", reflect.TypeOf(*m))
	(*m)["a"] = 44
	t.Log(*m, "\t", reflect.TypeOf((*m)["a"]))

	t.Log("========使用make创建Map=======")
	*m = make(map[string]int, 0)
	t.Log(*m, "\t", reflect.TypeOf(*m))
	(*m)["b"] = 55
	t.Log(*m, "\t", len(*m))

	t.Log("========性能差异=======")
	makeMapTime()
	newMapTime()
}

```

### 运行结果

```go
=== RUN   TestDiffMakeAndNew
    map_test.go:43: ========使用new创建Map=======
    map_test.go:45: &map[] 	 *map[string]int
    map_test.go:46: 0xc00000e038 	 **map[string]int
    map_test.go:47: map[] 	 map[string]int
    map_test.go:49: map[] 	 map[string]int
    map_test.go:51: map[a:44] 	 int
    map_test.go:53: ========使用make创建Map=======
    map_test.go:55: map[] 	 map[string]int
    map_test.go:57: map[b:55] 	 1
    map_test.go:59: ========性能差异=======
	使用make函数执行完成耗时： 4.940985678s
	使用new函数执行完成耗时： 4.796069742s
--- PASS: TestDiffMakeAndNew (9.74s)
```

### 辅助函数

```go
// newMapTime
func newMapTime() {
	m := new(map[string]int)
	*m = map[string]int{}
	start := time.Now() // 获取当前时间
	for i := 0; i < 10000000; i++ {
		(*m)[strconv.Itoa(i)] = i
	}
	elapsed := time.Now().Sub(start)
	fmt.Println("\t使用new函数执行完成耗时：", elapsed)
}

// makeMapTime
func makeMapTime() {
	m := make(map[string]int)
	start := time.Now() // 获取当前时间
	for i := 0; i < 10000000; i++ {
		m[strconv.Itoa(i)] = i
	}
	elapsed := time.Now().Sub(start)
	fmt.Println("\t使用make函数执行完成耗时：", elapsed)
}
```

## Tip 5 应该优先考虑哪些类型作为Map的key？

先说结论：优先选用**数值类型和指针类型**。如果非要选择**字符串类型的话，最好对键值的长度进行额外的约束。**

只从性能上看的话，**求哈希和判等操作的速度越快，对应的类型就越适合作为键类型。**

所有的基本类型、指针类型，以及数组类型、结构体类型和接口类型，Go 语言都有一套算法与之对应，其中就包含了哈希和判等。以求哈希的操作为例，**宽度**越小的类型速度通常越快。**宽度**是指类型单个值需要占用的字节数。比如，bool、int8和uint8类型的一个值需要占用的字节数都是1，因此这些类型的宽度就都是1。

注意，最好不要用高级数据类型作为Map的key，不仅仅是因为对它们的值求哈希，以及判等的速度较慢，更是因为在它们的值中存在变数。比如，对一个数组来说，我可以任意改变其中的元素值，但在变化前后，它却代表了两个不同的键值。

### 附录

[All data types in Golang with examples](https://golangbyexample.com/all-data-types-in-golang-with-examples/)

## Tip6 Map类型的Value实现并发

由于map操作不是原子性的，所以多个携程同时操作map有可能产生读写冲突，会触发panic。Go团队在[Why are map operations not defined to be atomic](https://golang.org/doc/faq#atomic_maps)对此进行了解释。

如果有需求可以参考第三方封装的concurrent map和Go1.9以后内置的`sync.Map`

###  sync.Map使用场景

> The Map type is optimized for two common use cases:
>
> 1. when the entry for a given key is only ever written once but read many times, as in caches that only grow.
> 2. when multiple goroutines read, write, and overwrite entries for disjoint sets of keys. In these two cases, use of a Map may **significantly** reduce lock contention compared to a Go map paired with a separate Mutex or RWMutex.

- 写少读多的情况
- 多goroutine，读、写、复写 不同数据的情况
- 多核心情况使用

### Concurrent.map使用场景

- 读写都相对频繁

### 附录

[concurrent map](https://github.com/orcaman/concurrent-map)

## Tip7 Map的key不能是哪些类型

Go 语言规范规定，在键类型的值之间必须可以施加操作符==和!=。换句话说，键类型的值必须要支持判等操作。由于函数类型、字典类型和切片类型的值并不支持判等操作，所以字典的键类型不能是这些类型。

另外，如果键的类型是接口类型的，那么键值的实际类型也不能是上述三种类型，否则在程序运行过程中会引发 panic

## Tip8 Map与JSON相互转换

我们经常会遇到JSON转换成Map或是Map转换为JSON的需求，这里推荐使用jsoniter替代原生的json包，其不仅支持原生的json，而且性能更快。

```go
import (
    ...
    jsoniter "github.com/json-iterator/go"
)

var json = jsoniter.ConfigCompatibleWithStandardLibrary


// JSONToMap return map and error
func JSONToMap(jsonStr string) (map[string]string, error) {
	m := make(map[string]string)
	err := json.Unmarshal([]byte(jsonStr), &m)
	if err != nil {
		fmt.Printf("Unmarshal with error: %+v\n", err)
		return nil, err
	}
	return m, nil
}

// MapToJSON return json string and error
func MapToJSON(m map[string]string) (string, error) {
	jsonByte, err := json.Marshal(m)
	if err != nil {
		fmt.Printf("Marshal with error: %+v\n", err)
		return "", nil
	}
	return string(jsonByte), nil
}
```

### 附录:

[json-iterator](https://github.com/json-iterator/go)

[json性能对比](https://jsoniter.com/benchmark.html)