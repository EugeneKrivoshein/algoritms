# Алгоритм Дейкстры

## Задача: Реализация алгоритма Дейкстры на языке Go

Описание задачи

Алгоритм Дейкстры используется для нахождения кратчайших путей от одной вершины до всех других вершин в графе, который имеет неотрицательные веса рёбер. Этот алгоритм широко используется в задачах оптимизации маршрутов, в сетевой инженерии и во многих других областях.

Задача для исполнения

Реализуйте алгоритм Дейкстры для нахождения кратчайшего пути от заданной начальной вершины до всех остальных вершин в взвешенном графе.

Определение структуры и методов

```go
package main

import (
    "fmt"
    "math"
)

type Edge struct {
    to   int
    cost int
}

type Graph struct {
    vertices int
    edges    [][]Edge
}

func NewGraph(vertices int) *Graph {
    return &Graph{
        vertices: vertices,
        edges:    make([][]Edge, vertices),
    }
}

func (g *Graph) AddEdge(from, to, cost int) {
    g.edges[from] = append(g.edges[from], Edge{to, cost})
}

func (g *Graph) Dijkstra(source int) []int {
    minDist := make([]int, g.vertices)
    visited := make([]bool, g.vertices)
    for i := range minDist {
        minDist[i] = math.MaxInt32
    }
    minDist[source] = 0

    for i := 0; i < g.vertices; i++ {
        u := -1
        for v := 0; v < g.vertices; v++ {
            if !visited[v] && (u == -1 || minDist[v] < minDist[u]) {
                u = v
            }
        }

        visited[u] = true
        for _, e := range g.edges[u] {
            if minDist[u]+e.cost < minDist[e.to] {
                minDist[e.to] = minDist[u] + e.cost
            }
        }
    }
    return minDist
}

func main() {
    graph := NewGraph(5)
    graph.AddEdge(0, 1, 10)
    graph.AddEdge(0, 2, 3)
    graph.AddEdge(1, 2, 1)
    graph.AddEdge(1, 3, 2)
    graph.AddEdge(2, 1, 4)
    graph.AddEdge(2, 3, 8)
    graph.AddEdge(2, 4, 2)
    graph.AddEdge(3, 4, 7)
    graph.AddEdge(4, 3, 9)

    distances := graph.Dijkstra(0)
    fmt.Println("The shortest distances from node 0 to all other nodes are:")
    for index, dist := range distances {
        fmt.Printf("Distance to %d: %d\n", index, dist)
    }
}
```

Задачи для исполнения

-Изучите предложенный код и убедитесь, что понимаете его логику.
-Протестируйте алгоритм на различных входных данных и различных графах, чтобы убедиться в его корректности и эффективности.
-Модифицируйте код, чтобы он возвращал не только расстояния, но и пути — это полезно для отображения конкретных маршрутов.

Практическое применение

Алгоритм Дейкстры чрезвычайно полезен в приложениях, где требуется находить оптимальные пути, например, в GPS-навигации, сетевом роутинге, планировании маршрутов в логистике и во многих других областях. Понимание этого алгоритма и умение его реализовать открывают
широкие возможности для создания сложных и высокоэффективных систем.