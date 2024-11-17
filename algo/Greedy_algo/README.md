# Жадные алгоритмы

## Задача: Реализация жадных алгоритмов на языке Go

Описание задачи
Жадные алгоритмы — это подход, который строит глобальное решение, принимая на каждом шаге локально оптимальное решение. Эти алгоритмы часто используются там, где нужно решить оптимизационную задачу. Одним из классических примеров использования жадных алгоритмов является поиск минимального остовного дерева (MST) в графе.

Задача для исполнения
Реализуйте жадный алгоритм для решения задачи о минимальном остовном дереве (MST), используя алгоритм Краскала.

## Реализация алгоритма Краскала в Go

```go 
package main

import (
    "fmt"
    "sort"
)

type Edge struct {
    start, end, weight int
}

type DisjointSet struct {
    parent, rank []int
}

func NewDisjointSet(vertexCount int) *DisjointSet {
    parent := make([]int, vertexCount)
    rank := make([]int, vertexCount)
    for i := range parent {
        parent[i] = i
    }
    return &DisjointSet{parent, rank}
}

func (set *DisjointSet) Find(v int) int {
    if set.parent[v] != v {
        set.parent[v] = set.Find(set.parent[v])
    }
    return set.parent[v]
}

func (set *DisjointSet) Union(x, y int) {
    rootX := set.Find(x)
    rootY := set.Find(y)
    if rootX != rootY {
        if set.rank[rootX] > set.rank[rootY] {
            set.parent[rootY] = rootX
        } else if set.rank[rootX] < set.rank[rootY] {
            set.parent[rootX] = rootY
        } else {
            set.parent[rootY] = rootX
            set.rank[rootX]++
        }
    }
}

func KruskalMST(vertices int, edges []Edge) []Edge {
    sort.Slice(edges, func(i, j int) bool {
        return edges[i].weight < edges[j].weight
    })

    mst := make([]Edge, 0)
    set := NewDisjointSet(vertices)

    for _, e := range edges {
        if set.Find(e.start) != set.Find(e.end) {
            set.Union(e.start, e.end)
            mst = append(mst, e)
        }
    }

    return mst
}

func main() {
    edges := []Edge{
        {0, 1, 10}, {0, 2, 6}, {0, 3, 5},
        {1, 3, 15}, {2, 3, 4},
    }
    mst := KruskalMST(4, edges)
    fmt.Println("Edges in MST:")
    for _, e := range mst {
        fmt.Printf("%d - %d: %d\n", e.start, e.end, e.weight)
    }
}
```

Задачи для исполнения

-Понимание и реализация алгоритма Краскала для нахождения минимального остовного дерева.
-Протестировать алгоритм на различных графах для убеждения в его правильности и эффективности.
-Добавьте функциональность для визуализации графа и его MST, если это возможно, для лучшего понимания процесса работы алгоритма.

Практическое применение

Жадные алгоритмы часто используются в оптимизационных задачах, таких как расписание, оптимизация ресурсов, сетевое проектирование и многие другие. Понимание этих алгоритмов открывает широкие возможности для разработки эффективных решений в различных областях.

