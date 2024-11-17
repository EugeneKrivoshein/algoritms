# Стеки и очереди

## Задача: Реализация стеков и очередей на языке Go

Описание задачи

Стеки и очереди являются фундаментальными структурами данных, используемыми для управления объектами в определённом порядке. Стек обрабатывает элементы по принципу LIFO (Last In, First Out — последний пришел, первый ушел), в то время как очередь работает по принципу FIFO (First In, First Out — первый пришел, первый ушел). Эти структуры данных широко применяются в алгоритмах обработки данных, планировании задач, выполнении алгоритмов парсинга и многих других задачах.

Реализация стека

Определение структуры и методов

```go
package main

import "fmt"

type Stack struct {
    elements []int
}

// Push добавляет элемент в стек
func (s *Stack) Push(value int) {
    s.elements = append(s.elements, value)
}

// Pop удаляет верхний элемент из стека и возвращает его
func (s *Stack) Pop() (int, bool) {
    if len(s.elements) == 0 {
        return 0, false
    }
    index := len(s.elements) - 1
    element := s.elements[index]
    s.elements = s.elements[:index]
    return element, true
}

func main() {
    stack := &Stack{}
    stack.Push(1)
    stack.Push(2)
    stack.Push(3)
    fmt.Println(stack.Pop()) // Вывод: 3, true
    fmt.Println(stack.Pop()) // Вывод: 2, true
    fmt.Println(stack.Pop()) // Вывод: 1, true
    fmt.Println(stack.Pop()) // Вывод: 0, false
}
```

Реализация очереди

Определение структуры и методов

```go
package main

import "fmt"

type Queue struct {
    elements []int
}

// Enqueue добавляет элемент в конец очереди
func (q *Queue) Enqueue(value int) {
    q.elements = append(q.elements, value)
}

// Dequeue удаляет элемент из начала очереди и возвращает его
func (q *Queue) Dequeue() (int, bool) {
    if len(q.elements) == 0 {
        return 0, false
    }
    element := q.elements[0]
    q.elements = q.elements[1:]
    return element, true
}

func main() {
    queue := &Queue{}
    queue.Enqueue(1)
    queue.Enqueue(2)
    queue.Enqueue(3)
    fmt.Println(queue.Dequeue()) // Вывод: 1, true
    fmt.Println(queue.Dequeue()) // Вывод: 2, true
    fmt.Println(queue.Dequeue()) // Вывод: 3, true
    fmt.Println(queue.Dequeue()) // Вывод: 0, false
}
```

Задачи для исполнения

-Изучите и поймите предложенный код для стека и очереди.
Протестируйте обе структуры на разных наборах данных.
-Модифицируйте стек и очередь, добавив функциональность для обработки разных типов данных (не только int).
-Реализуйте дополнительные функции, такие как Peek для стека и очереди, которые позволяют посмотреть на элемент в начале структуры без его удаления.

Практическое применение

Стеки и очереди используются во множестве компьютерных наук и приложений программирования, включая выполнение обратной польской нотации в калькуляторах, управление процессами в операционных системах, алгоритмы парсинга выражений и многое другое. Понимание и умение реализовывать эти структу

ры данных улучшает навыки решения проблем и программирования.