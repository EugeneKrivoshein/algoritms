# КУЧИ

## Задача: Реализация кучи (Heap) на языке Go

Описание задачи

Куча — это специализированная структура данных в виде дерева, которая удовлетворяет свойству кучи: каждый родительский узел больше или равен (в случае максимальной кучи) или меньше или равен (в случае минимальной кучи) своих дочерних узлов. Это свойство делает кучу идеальным кандидатом для реализации приоритетных очередей, где требуется быстрый доступ к наибольшему (или наименьшему) элементу.

Задача для исполнения

Реализуйте максимальную кучу в Go, поддерживая операции вставки элемента, извлечения максимального элемента и получения максимального элемента без его удаления.

Определение структуры и методов

```go
package main

import (
    "fmt"
)

// MaxHeap структура представляющая максимальную кучу
type MaxHeap struct {
    array []int
}

// Insert добавляет элемент в кучу
func (h *MaxHeap) Insert(key int) {
    h.array = append(h.array, key)
    h.maxHeapifyUp(len(h.array) - 1)
}

// ExtractMax извлекает и возвращает максимальный элемент из кучи
func (h *MaxHeap) ExtractMax() int {
    if len(h.array) == 0 {
        fmt.Println("Heap is empty")
        return -1
    }
    max := h.array[0]
    // Перемещаем последний элемент в корень
    h.array[0] = h.array[len(h.array)-1]
    h.array = h.array[:len(h.array)-1]
    h.maxHeapifyDown(0)
    return max
}

// Peek возвращает максимальный элемент без его удаления
func (h *MaxHeap) Peek() int {
    if len(h.array) == 0 {
        fmt.Println("Heap is empty")
        return -1
    }
    return h.array[0]
}

// maxHeapifyUp для восстановления свойств максимальной кучи после вставки
func (h *MaxHeap) maxHeapifyUp(index int) {
    for h.array[index] > h.array[parent(index)] {
        h.swap(index, parent(index))
        index = parent(index)
    }
}

// maxHeapifyDown для восстановления свойств максимальной кучи после извлечения
func (h *MaxHeap) maxHeapifyDown(index int) {
    lastIndex := len(h.array) - 1
    l, r, largest := left(index), right(index), index
    if l <= lastIndex && h.array[l] > h.array[largest] {
        largest = l
    }
    if r <= lastIndex && h.array[r] > h.array[largest] {
        largest = r
    }
    if largest != index {
        h.swap(index, largest)
        h.maxHeapifyDown(largest)
    }
}

// Вспомогательные функции для получения индекса родителя, левого и правого потомка
func parent(i int) int { return (i - 1) / 2 }
func left(i int) int   { return 2*i + 1 }
func right(i int) int  { return 2*i + 2 }

// swap меняет местами элементы
func (h *MaxHeap) swap(i1, i2 int) {
    h.array[i1], h.array[i2] = h.array[i2], h.array[i1]
}

func main() {
    heap := &MaxHeap{}
    heap.Insert(3)
    heap.Insert(2)
    heap.Insert(15)
    heap.Insert(5)
    heap.Insert(4)
    heap.Insert(45)
    fmt.Println(heap.ExtractMax()) // 45
    fmt.Println(heap.Peek())       // 15
    fmt.Println(heap.ExtractMax()) // 15
}
```

Задачи для исполнения

-Изучите и поймите предложенный код, особенно логику функций maxHeapifyUp и maxHeapifyDown.
-Протестируйте кучу на разных наборах данных, проверяя корректность всех операций.
-Расширьте функциональность, добавив методы для удаления конкретного элемента или изменения приоритета элемента в куче.

Практическое применение

Кучи широко используются в программировании для задач, требующих частого доступа к наибольшему или наименьшему элементу, в том числе для управления памятью, планирования задач в операционных системах, симуляции очередей с приоритетами, сетевого трафика и многого другого. Понимание и умение реализовывать кучи значительно повышает навыки разработчика в работе со сложными структурами данных.