# Алгоритм Краскала и Прима

## Задача: Реализация алгоритмов Краскала и Прима на языке Go

Описание задачи

Алгоритмы Краскала и Прима используются для нахождения минимального остовного дерева (MST) в связном взвешенном графе. Это дерево соединяет все вершины графа, минимизируя суммарный вес рёбер, и не содержит циклов. Минимальные остовные деревья используются во множестве приложений, включая проектирование сетевых соединений, планирование дорог, и во многих других областях, где требуется минимизация стоимости инфраструктуры.

Алгоритм Краскала

-Инициализируйте лес, состоящий из одиночных вершин графа.
-Сортируйте все рёбра по весу в порядке возрастания.
-Добавляйте рёбра к лесу, пропуская те, которые формируют цикл, пока не соедините все вершины.

Алгоритм Прима

-Начните с произвольной вершины и добавьте её в остовное дерево.
-Повторяйте следующие шаги до включения всех вершин в остовное дерево:
-Из множества рёбер, которые соединяют дерево с вершинами не в дереве, выберите ребро с наименьшим весом и добавьте его в дерево.

## Реализация алгоритма Краскала в Go

```go
package main

import (
    "fmt"
    "sort"
)

type edge struct {
    start, end, cost int
}

type disjointSet struct {
    parent, rank []int
}

func newDisjointSet(size int) *disjointSet {
    d := &disjointSet{
        parent: make([]int, size),
        rank:   make([]int, size),
    }
    for i := range d.parent {
        d.parent[i] = i
    }
    return d
}

func (d *disjointSet) find(x int) int {
    if d.parent[x] != x {
        d.parent[x] = d.find(d.parent[x])
    }
    return d.parent[x]
}

func (d *disjointSet) union(x, y int) {
    rootX := d.find(x)
    rootY := d.find(y)
    if rootX != rootY {
        if d.rank[rootX] > d.rank[rootY] {
            d.parent[rootY] = rootX
        } else if d.rank[rootX] < d.rank[rootY] {
            d.parent[rootX] = rootY
        } else {
            d.parent[rootY] = rootX
            d.rank[rootX]++
        }
    }
}

func kruskal(edges []edge, vertices int) []edge {
    mst := []edge{}
    sort.Slice(edges, func(i, j int) bool {
        return edges[i].cost < edges[j].cost
    })
    ds := newDisjointSet(vertices)

    for _, e := range edges {
        if ds.find(e.start) != ds.find(e.end) {
            mst = append(mst, e)
            ds.union(e.start, e.end)
        }
    }
    return mst
}

func main() {
    edges := []edge{
        {0, 1, 10}, {0, 2, 6}, {0, 3, 5},
        {1, 3, 15}, {2, 3, 4},
    }
    mst := kruskal(edges, 4)
    fmt.Println("Edges in MST:")
    for _, e := range mst {
        fmt.Printf("%d - %d: %d\n", e.start, e.end, e.cost)
    }
}
```

Задачи для исполнения

-Понимание и реализация алгоритма Краскала. 
-Реализуйте алгоритм Прима, используя подход, аналогичный представленному для Краскала. 
-Сравните эффективность и результаты обоих алгоритмов на различных наборах данных.

Практическое применение

Понимание и умение реализовать алгоритмы Краскала и Прима открывает возможности для решения сложных задач оптимизации в реальных приложениях, таких как минимизация расходов на инфраструктуру, оптимизация сетевых маршрутов и многое другое.
