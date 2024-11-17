# Merge sort

## Реализуйте конкурентное решение merge sort (сортировка слиянием), используя горутины и каналы.

В качестве опорной точки можно взять эту последовательную реализацию:

```go
package main 

import "fmt"

func Merge(left, right [] int) [] int{  
	merged := make([] int, 0, len(left) + len(right))  
	
	for len(left) > 0 || len(right) > 0{  
		if len(left) == 0 {  
			return append(merged,right...)  
		} else if len(right) == 0 {  
			return append(merged,left...)  
		} else if left[0] < right[0] {  
			merged = append(merged, left[0])  
			left = left[1:]  
		} else{  
			merged = append(merged, right [0])  
			right = right[1:]  
		}  
	}
	
	return merged  
}

func MergeSort(data [] int) [] int {  
	if len(data) <= 1 {  
		return data  
	}
	
	mid := len(data)/2  
	left := MergeSort(data[:mid])  
	right := MergeSort(data[mid:])  
	return Merge(left,right)  
}

func main(){  
	data := [] int{9,4,3,6,1,2,10,5,7,8}  
	fmt.Printf("%v\n%v\n", data, MergeSort(data))  
}
```
Решение

```go
package main

import "fmt"

func Merge(left, right [] int) [] int{  
	merged := make([] int, 0, len(left) + len(right))
	
	for len(left) > 0 || len(right) > 0 {  
		if len(left) == 0 {  
			return append(merged,right...)  
		} else if len(right) == 0 {  
			return append(merged,left...)  
		} else if left[0] < right[0] {  
			merged = append(merged, left[0])  
			left = left[1:]  
		} else {  
			merged = append(merged, right [0])  
			right = right[1:]  
		}  
	}
	
	return merged  
}

func MergeSort(data [] int) [] int {  
	if len(data) <= 1 {  
		return data  
	}
	
	done := make(chan bool)  
	mid := len(data)/2  
	var left [] int
	
	go func(){  
		left = MergeSort(data[:mid])  
		done <- true  
	}()
	
	right := MergeSort(data[mid:])  
	<-done  
	return Merge(left,right)  
}

func main(){  
	data := [] int{9,4,3,6,1,2,10,5,7,8}  
	fmt.Printf("%v\n%v\n", data, MergeSort(data))  
}
```
При сортировке слиянием мы сначала рекурсивно делим массив на две части: правую (right) и левую (left), и вызываем функцию MergeSort для каждой из них.

Нужно сделать так, чтобы функция Merge(left, right) выполнялась после завершения обоих рекурсивных вызовов. То есть, и left, и right должны быть обновлены до выполнения Merge(left, right).

Для этого на строке 26 мы вводим канал типа bool и отправляем в него true сразу после выполнения left = MergeSort(data[:mid]) на строке 32.

Инструкция <-done блокирует выполнение на строке 35 до завершения функции Merge(left, right), чтобы она не выполнялась до завершения горутины. После завершения горутины и получения true в канале done, код переходит к инструкции Merge(left, right) на строке 36.

