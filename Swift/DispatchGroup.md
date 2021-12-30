# Dispatch group

Single 단위로 모니터하는 task group

group을 사용하면 task 집합을 집계하고 그룹의 동작을 동기화할 수 있습니다.

여러 작업 항목을 그룹에 연결하고 동일한 대기열 또는 다른 대기열에서 비동기식으로 실행되도록 예약할 수 있습니다. 모든 작업 항목의 실행이 완료되면 그룹이 완료 핸들러를 실행합니다. 그룹의 모든 태스크가 실행이 완료될 때까지 동기적으로 기다릴 수도 있습니다.

<br>

### **Creating a Dispatch Group**

블록 객체들을 할당할 수 있는 DispatchGroup을 만드는 방법은 init()을 사용합니다.

```swift
let group = DispatchGroup()
```

<br>

### **Updating the Group automatically**

enter, leave를 사용하지 않는 경우 자동으로 작업을 그룹에 연결한다.

```swift
DispatchQueue.global().async(group: group) { }
DispatchQueue.main.async(group: group) { }
```

<br>

### **Updating the Group Manually**

enter, leave를 사용하여 직접 작업을 그룹에 연결한다.

<aside>
💡 근데 궁금한건 왜 leave를 코드 내부에 넣는 것일까..?

</aside>

<aside>
💡 왜 sync는 그룹에 넣을 수 없는 것일까...?

</aside>

```swift
group.enter()
DispatchQueue.global().async() {
    group.leave()
}
        
group.enter()
DispatchQueue.main.async() {
    group.leave()
}
```

<br>

### **Adding a Completion Handler**

notify() 를 이용해 group의 모든 작업 실행이 완료 될 때 queue에 블록을 제출하는 것을 스케줄합니다.

추가로 notify() 내에 속성을 지정하여 해당 속성을 가진 블록을 제출할 수도 있습니다.

<aside>
💡 내부에 정의되는 queue는 어떤 큐를 넣어야하는지... 모르겠다..

</aside>

```swift
group.notify(queue: DispatchQueue.main) {
    print("완료")
}
```

<br>

### **Waiting for Tasks to Finish Executing**

이전에 제출한 작업이 완료될 때까지 동기식으로 기다립니다.

또한, 시간을 설정하여 시간 초과기간이 경과하기 전에 작업이 완료되지 않으면 반환합니다.

```swift
// 작업이 완료될 시에
group.wait()

// 작업이 완료되기 까지 기다리는 시간 설정
group.wait(timeout: 10)
```
