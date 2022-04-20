## go slice



### slice结构定义

```
type slice struct {
  array unsafe.Pointer//指向底层数组的指针
  len   int//切片的长度
  cap   int//切片的容量
}
```

### slice扩容

当原slice的cap小于1024时，新slice的cap变为原来的2倍；原slice的cap大于1024时，新slice变为原来的1.25倍

我们知道golang中内存分配是根据对象大小来配不同的mspan，为了避免造成过多的内存碎片，slice在扩容中需要对扩容后的cap容量进行内存对齐的操作



### slice截取

```
package main

import "fmt"

func main() {
  slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
  s1 := slice[2:5]
  s2 := s1[2:7]
  fmt.Printf("len=%-4d cap=%-4d slice=%-1v \n", len(slice), cap(slice), slice)
  fmt.Printf("len=%-4d cap=%-4d s1=%-1v \n", len(s1), cap(s1), s1)
  fmt.Printf("len=%-4d cap=%-4d s2=%-1v \n", len(s2), cap(s2), s2)
}
```

```
len=10   cap=10   slice=[0 1 2 3 4 5 6 7 8 9] 
len=3    cap=8    s1=[2 3 4] 
len=5    cap=6    s2=[4 5 6 7 8]
```

实际截取的是底层数组



#### slice深拷贝

```

func main() {

  // Creating slices
  slice1 := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
  var slice2 []int
  slice3 := make([]int, 5)

  // Before copying
  fmt.Println("------------before copy-------------")
  fmt.Printf("len=%-4d cap=%-4d slice1=%v\n", len(slice1), cap(slice1), slice1)
  fmt.Printf("len=%-4d cap=%-4d slice2=%v\n", len(slice2), cap(slice2), slice2)
  fmt.Printf("len=%-4d cap=%-4d slice3=%v\n", len(slice3), cap(slice3), slice3)


  // Copying the slices
  copy_1 := copy(slice2, slice1)
  fmt.Println()
  fmt.Printf("len=%-4d cap=%-4d slice1=%v\n", len(slice1), cap(slice1), slice1)
  fmt.Printf("len=%-4d cap=%-4d slice2=%v\n", len(slice2), cap(slice2), slice2)
  fmt.Println("Total number of elements copied:", copy_1)
}
```

copy函数为slice提供了深拷贝能力，但是需要在拷贝前申请内存空间



#### slice是值传递，但是底层数组是引用传递







































参考

[https://blog.csdn.net/ITqingliang/article/details/104605781](https://blog.csdn.net/ITqingliang/article/details/104605781)

