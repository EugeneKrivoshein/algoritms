# Хэш таблицы

## Задача: Реализация хеш-таблицы на языке Go

Описание задачи
Хеш-таблицы являются одной из основных структур данных, используемых в программировании для организации данных таким образом, чтобы обеспечивать быстрый поиск, вставку и удаление элементов.

Они работают на принципе ассоциации ключ-значение, где каждый ключ преобразуется в индекс массива с помощью хеш-функции. Это делает доступ к элементам почти мгновенным и очень эффективным даже для больших объемов данных. Хеш-таблицы широко используются в реализации баз данных, кэшировании, уникальных наборах данных и многом другом.

Задача для исполнения
Разработайте простую реализацию хеш-таблицы в Go, которая поддерживает операции вставки, удаления и поиска элементов.

### Определение структуры и методов

```go
package main

import (
    "fmt"
)

const ArraySize = 7

// HashTable структура для хеш-таблицы
type HashTable struct {
    array [ArraySize]*bucket
}

// bucket структура, представляющая список для разрешения коллизий
type bucket struct {
    head *bucketNode
}

// bucketNode структура узла в списке
type bucketNode struct {
    key   string
    value int
    next  *bucketNode
}

// Insert добавляет пару ключ-значение в хеш-таблицу
func (h *HashTable) Insert(key string, value int) {
    index := hash(key)
    h.array[index].insert(key, value)
}

// Search возвращает значение по ключу в хеш-таблице
func (h *HashTable) Search(key string) (int, bool) {
    index := hash(key)
    return h.array[index].search(key)
}

// Delete удаляет элемент по ключу из хеш-таблицы
func (h *HashTable) Delete(key string) {
    index := hash(key)
    h.array[index].delete(key)
}

// hash простая хеш-функция
func hash(key string) int {
    sum := 0
    for _, v := range key {
        sum += int(v)
    }
    return sum % ArraySize
}

// insert добавляет элемент в список
func (b *bucket) insert(key string, value int) {
    if !b.searchKey(key) {
        newNode := &bucketNode{key: key, value: value}
        newNode.next = b.head
        b.head = newNode
    } else {
        // Обновляем значение, если ключ уже существует
        currNode := b.head
        for currNode.key != key {
            currNode = currNode.next
        }
        currNode.value = value
    }
}

// search ищет ключ в списке
func (b *bucket) search(key string) (int, bool) {
    currNode := b.head
    for currNode != nil {
        if currNode.key == key {
            return currNode.value, true
        }
        currNode = currNode.next
    }
    return 0, false
}

// delete удаляет ключ из списка
func (b *bucket) delete(key string) {
    if b.head.key == key {
        b.head = b.head.next
        return
    }
    prevNode := b.head
    for prevNode.next.key != key {
        prevNode = prevNode.next
    }
    prevNode.next = prevNode.next.next
}

func main() {
    hashTable := &HashTable{}
    for i := range hashTable.array {
        hashTable.array[i] = &bucket{}
    }

    fmt.Println("Hash Table Created")
    hashTable.Insert("name", 123)
    hashTable.Insert("age", 456)
    fmt.Println("name:", hashTable.Search("name"))
    fmt.Println("age:", hashTable.Search("age"))

    hashTable.Delete("name")
    fmt.Println("name:", hashTable.Search("name"))
}
```

Задачи для исполнения

-Изучите и поймите предложенный код, обратите внимание на то, как функции insert, search и delete работают с коллизиями.
-Запустите код и протестируйте функциональность хеш-таблицы на различных входных данных.
-
Расширьте функциональность хеш-таблицы, добавив реализацию автоматического масштабирования таблицы при высокой загрузке (rehashing) для поддержания эффективности операций.

Практическое применение

Понимание и умение реализовать хеш-таблицы является критически важным навыком для разработчиков программного обеспечения, особенно тех, кто работает с базами данных, кэшированием, алгоритмами обнаружения дубликатов и многими другими задачами, где требуется быстрый поиск данных.