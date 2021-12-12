# 단순 연결 리스트 (Singly Linked List)

### LinkedList 구현 과정

이번 프로젝트에서 Queue는 Dequeue, 즉 양쪽 끝에서 삽입, 삭제가 가능한 자료구조를 사용할 필요 없이 일반적인 FIFO만을 지원하는 Queue를 구현해도 작동에 문제 없겠다는 판단이 들었습니다.  

Queue는 FIFO으로 첫번째 노드를 삭제하고 반환하는 기능과 마지막 노드에 데이터를 삽입하는 기능만 필요로 합니다. 그렇다면 탐색이나 조회의 기능이  중요하지 않고 첫번째 노드와 마지막 노드에 접근하는 방법이 중요하다고 생각했습니다.

LinkedList는 순차적으로 data에 접근하여 탐색, 조회 시 시간복잡도가 증가하는 단점이있지만 첫번째 노드와 마지막 노드에 접근하는 시간복잡도에서 유리합니다. 

결과적으로 LinkedList를 구현하였습니다. 초반에는 단순 연결리스트로 구현을 했는데요

```swift
struct LinkedList {
    private var head: Node?

    mutating func append(data: String?) {
        if head == nil {
            self.head = Node(data: data)
            return
				}

        var node = head
        while node?.next != nil {
            node = node?.next
				}
		    node?.next = Node(data: data)
    }
```

이때, append시에 head 노드부터 맨 끝 노드까지 탐색해야한다는 단점이 있었습니다.

이렇게 구현하면 linkedlist 사용하는 이유가 없다고 생각하여 수정했습니다.

tail 프로퍼티를 이용하여 append 로직을 수정하였습니다.

```swift
struct LinkedList {
    private var head: Node?
    private var tail: Node?
    
    mutating func append(data: String?) {
        let newNode = Node(data: data)
        
        if tail != nil {
            tail?.next = newNode
        }
        tail = newNode
        
        if head == nil {
            self.head = tail
        }
    }
```
