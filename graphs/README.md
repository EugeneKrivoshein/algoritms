# Графы

## Задача: Реализация графа на языке Go

Описание задачи

Графы являются мощным инструментом для моделирования различных структур и процессов в компьютерных науках и других областях, таких как сетевые соединения, оптимизация маршрутов, социальные сети и многие другие. Они состоят из вершин (или узлов) и рёбер, соединяющих эти вершины.

Задача для исполнения

Разработайте структуру данных для представления графа и основные операции для работы с ним, такие как добавление вершин, добавление рёбер и обход графа.

Определение структуры и методов

```go 
package main

import "fmt"

// Graph структура, представляющая граф
type Graph struct {
    vertices []*Vertex
}

// Vertex структура, представляющая вершину графа
type Vertex struct {
    Key int
    Adjacent []*Vertex
}

// AddVertex добавляет вершину в граф
func (g *Graph) AddVertex(k int) {
    if contains(g.vertices, k) {
        err := fmt.Errorf("vertex %v not added because it is an existing key", k)
        fmt.Println(err.Error())
        return
    }
    g.vertices = append(g.vertices, &Vertex{Key: k})
}

// AddEdge добавляет ребро между двумя вершинами в графе
func (g *Graph) AddEdge(from, to int) {
    // Получаем вершины
    fromVertex := g.getVertex(from)
    toVertex := g.getVertex(to)
    // Проверяем, что вершины существуют
    if fromVertex == nil || toVertex == nil {
        err := fmt.Errorf("invalid edge (%v->%v)", from, to)
        fmt.Println(err.Error())
        return
    }
    // Проверяем, что ребро не существует
    if contains(fromVertex.Adjacent, to) {
        err := fmt.Errorf("existing edge (%v->%v)", from, to)
        fmt.Println(err.Error())
        return
    }
    // Добавляем ребро
    fromVertex.Adjacent = append(fromVertex.Adjacent, toVertex)
}

// Помощник для проверки существования вершины
func contains(s []*Vertex, k int) bool {
    for _, v := range s {
        if k == v.Key {
            return true
        }
    }
    return false
}

// Помощник для получения вершины по ключу
func (g *Graph) getVertex(k int) *Vertex {
    for _, v := range g.vertices {
        if v.Key == k {
            return v
        }
    }
    return nil
}

func main() {
    graph := &Graph{}
    for i := 1; i <= 5; i++ {
        graph.AddVertex(i)
    }
    graph.AddEdge(1, 2)
    graph.AddEdge(1, 3)
    graph.AddEdge(2, 3)
    graph.AddEdge(4, 2)
    graph.AddEdge(5, 2)
    graph.AddEdge(3, 5)

    // Выводим граф
    for _, v := range graph.vertices {
        fmt.Printf("Vertex %v : ", v.Key)
        for _, v := range v.Adjacent {
            fmt.Printf("%v ", v.Key)
        }
        fmt.Println()
    }
}

```

Задачи для исполнения

-Изучите и поймите предложенный код, особенно как графы представляются и как функции работают.
-Добавьте функцию обхода графа, такую как поиск в глубину (DFS) или поиск в ширину (BFS).
-Реализуйте функции удаления вершин и рёбер из графа.
Расширьте функциональность, позволяя создавать направленные и ненаправленные графы.

Практическое

применение Графы используются для решения разнообразных задач, от маршрутизации в сетях до анализа социальных сетей и многого другого. Понимание и умение работать с графами открывают множество возможностей для создания сложных и эффективных алгоритмов в самых разных приложениях.