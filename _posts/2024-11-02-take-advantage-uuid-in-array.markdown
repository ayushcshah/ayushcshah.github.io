---
layout: post
title:  "Swift Arrays Reimagined: Fast Lookups and Unique Entries with DictionaryArray"
date:   2024-11-02 21:27:12 -0600
category: [optimize, array, swift, uuid, data-structure, algorithms]
---
Arrays are everywhere in development—whether it’s a list of products, news articles, books, or user profiles, arrays make it easy to organize and access data in a linear fashion. But what happens when you need to frequently search, update, or delete elements in a large array by a unique identifier, like a UUID?

In iOS development, performing operations like `first(where:)` or `removeAll(where:)` on an array with a unique identifier parameter is an `O(n)` operation. For large arrays, this can be inefficient and may impact performance, especially when these operations are frequent. So, is there a more optimal way to manage arrays of uniquely identified items?

Let’s explore how using a custom `DictionaryArray` structure can improve performance for these cases by combining the flexibility of arrays with the fast lookup power of dictionaries.

Problem Overview
---
The main drawback of traditional arrays in Swift is that searching, updating, and deleting items by a unique identifier generally requires iterating through the entire array to find a match. This means that each operation has `O(n)` complexity, where n is the number of items in the array. For example:

```swift
let product = products.first(where: { $0.id == targetId })
```


Assumptions
---
For this solution, we’ll make the following assumptions:

1. Each element in the array has a unique identifier (UUID).
2. We frequently need to perform find, delete, and insert operations.
3. The array can contain up to 1,000 items, so space efficiency is less critical.

Solution: Introducing DictionaryArray
---
To make these operations more efficient, we can use a dictionary to store the elements, with each item’s `UUID` as the key. This will allow us to access any item in constant time, `O(1)`. Let’s call this custom structure `DictionaryArray`.

By structuring our data this way, we can enjoy the best of both worlds:
- Fast lookups (thanks to the dictionary).
- Enforcement of unique keys, preventing duplicate entries.

Let’s start by defining a protocol to ensure every item has a unique identifier.

```swift
protocol DictonaryArrayProviding: Hashable {
    var id: UUID { get set }
}
```

This protocol makes sure every element has a unique ID that can act as a key. Now, let’s create a `Product` struct that conforms to this protocol.

```swift
struct Product: DictonaryArrayProviding {
    var id: UUID
    var name: String
    var size: String
    var price: Double
    var children: [Product]
}
```

Next, we’ll implement the `DictionaryArray` struct. This struct stores elements as a dictionary with UUID keys, allowing us to add, retrieve, update, and delete items in constant time, `O(1)`.

```swift
enum DictionaryArrayError: Error {
    case duplicateKey
}

struct DictionaryArray<T: DictionaryArrayProviding> {
    private var elements: [UUID: T] = [:]
    
    init(elements: [UUID : T]) {
        self.elements = elements
    }
    
    mutating func append(element: T) throws {
        guard elements[element.id] == nil else {
            throw DictionaryArrayError.duplicateKey
        }
        elements[element.id] = element
    }
    
    mutating func append(elements: [T]) throws {
        try elements.forEach { element in
            try append(element: element)
        }
    }
    
    func fetch(id: UUID) -> T? {
        return elements[id]
    }
    
    mutating func remove(id: UUID) {
        elements.removeValue(forKey: id)
    }
    
    mutating func update(id: UUID, data: T) {
        elements[id] = data
    }
    
    func all() -> [T] {
        return Array(elements.values)
    }
}

```
In this `DictionaryArray` struct:

1. `append(element:)` checks for duplicate keys before adding an item. If the key exists, it throws a duplicateKey error.
2. `fetch(id:)` retrieves an item by its ID in  `O(1)`time.
3. `remove(id:)` deletes an item by ID in `O(1)` time.
4. `update(id:data:)` replaces an item in `O(1)` time.
5. `all()` returns all elements as an array, although they aren’t guaranteed to be in any specific order.

> This approach is both efficient and straightforward, allowing us to leverage the unique identifier to optimize performance. By storing elements in a dictionary, we bypass the `O(n)` complexity of traditional array searches. This means faster data manipulation—even with frequent insertions and deletions—without sacrificing readability or usability.

> A smart choice for cases with unique identifiers, `DictionaryArray` offers both the speed of a dictionary and the ease of an array-like structure!

Usage Example
---
Now, let’s see how this `DictionaryArray` can be used in practice. Imagine we have a catalog with products that each have a unique UUID.
``` swift
let awesomeTshirt = Product(
    id: UUID(),
    name: "Awesome TShirt",
    size: "M",
    price: 10,
    children: []
)

let awesomeJeans = Product(
    id: UUID(),
    name: "Awesome Jeans",
    size: "L",
    price: 20,
    children: []
)

var dictionaryArray = DictionaryArray<Product>(elements: [:])

do {
    try dictionaryArray.append(element: awesomeTshirt)
    try dictionaryArray.append(element: awesomeJeans)
} catch DictionaryArrayError.duplicateKey {
    print("Error: Attempted to add a duplicate item.")
}

if let fetchedProduct = dictionaryArray.fetch(id: awesomeJeans.id) {
    print("Fetched product: \(fetchedProduct.name)")
}

dictionaryArray.all().forEach { product in
    print("Product name: \(product.name)")
}

```

In this example:

1. We create two Product instances and add them to dictionaryArray.
2. We fetch a product by its ID and print its name.
3. We retrieve all items with all() and print each name.

Observations and Potential Improvements
---
While `DictionaryArray` improves the efficiency of finding, updating, and deleting items, there are a few trade-offs:
- Order of Elements: Unlike arrays, dictionaries do not maintain a specific order. If preserving order is important, you could consider adding a secondary array to track order, but that would increase complexity.
- Edge Case Handling: We’ve ensured unique entries by throwing an error on duplicate keys, but this error handling could be expanded further based on use cases. For example, you could decide to update an existing entry if a duplicate is detected, depending on the app’s requirements.

Conclusion
---
The `DictionaryArray` structure is a powerful alternative to traditional arrays, offering the efficiency of dictionaries with the usability of arrays. It enables `O(1)` time complexity for lookups, insertions, updates, and deletions by using unique identifiers, making it ideal for collections where each element has a unique key and where fast access is crucial. This is particularly useful in scenarios like product catalogs, user management, and inventory systems, where large data sets benefit from efficient operations.

While `Set` also ensures uniqueness and provides `O(1)` operations for checking existence, insertion, and removal, it lacks direct key-based retrieval. `DictionaryArray`, on the other hand, allows retrieval by a specific identifier (UUID), making it more versatile when unique elements need to be frequently accessed by their unique keys. If the use case only requires maintaining unique entries without the need for indexed access, `Set` is a simpler and lighter option.

Overall, `DictionaryArray` is a valuable addition to the Swift toolkit, particularly when unique identifiers are involved and frequent, efficient lookups and updates are essential. It effectively combines the benefits of arrays, dictionaries, and sets, offering both speed and functionality for data that must be uniquely identified.

