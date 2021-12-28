# Generic where 절

type constraints을 사용하면 제네릭 함수, subscript, type 과 연관된 타입 파라미터의  요구사항을 정의할 수 있습니다.

- generic where 절을 사용하여 associated type이 특정 프로토콜을 준수를 요구할 수 있습니다.
- 특정 타입 파라미터와 associated type이 동일해야 함을 요구할 수 있습니다.

 제네릭 where 절은 where 키워드로 시작하고 associated type에 대한 제약 조건 또는 타입과 associated type 간의 동일 관계가 뒤 따른다. type 또는 func 본문의 여는 중괄호 바로 앞에 일반 where 절을 씁니다.

<br>

### **Generic Where Clauses**

C1.item과 C2.item이 동일하다는 제약조건과 c1.item 은 Equatable을 채택해야함을 명시합니다.

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {
```

<br>

### **Extensions with a Generic Where Clause**

extension에서도 where을 이용하여 제약을 줄 수 있다.

아래 코드는 stack 내 Element 타입은 equatable을 채택해야 함을 명시합니다.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
```

추가로 비교연산자를 이용해서 더 구체적인 타입으로 제약을 줄 수 있습니다.

```swift
extension Container where Item == Double {
    func average() -> Double {
```

<br>

### **Contextual Where Clauses**

이미 Generic type의 컨텍스트에서 작업 중인 경우 고유한 Generic type 제약이 없는 선언부에서 where절을 사용할 수 있습니다. 

```swift
extension Container {
    func average() -> Double where Item == Int {
        var sum = 0.0
```

<br>

### **Associated Types with a Generic Where Clause**

associated type에서 where절을 사용할 수 있습니다.

```swift
associatedtype Iterator: IteratorProtocol where Iterator.Element == Item

func makeIterator() -> Iterator
```

<br>

### Generic Subscripts

서브스크립트는 제네릭일 수 있으며, 제네릭 where 절을 사용할 수 있습니다. 

subscript 뒤에 <> 안에 placeholer를 작성하고 본문의 중괄호 앞에 where 절을 작성합니다.

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
```
