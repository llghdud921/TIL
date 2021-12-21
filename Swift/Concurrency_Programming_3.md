# 동시성 프로그래밍 Concurrency Programing (3)

<br>

## DispatchGroup

Single 단위로 모니터링하는 작업 그룹입니다.

Group을 사용하면 작업의 집합을 집계하고 Group에서 동작을 동기화할 수 있습니다. 여러 작업 항목을 그룹에 연결하고 동일한 대기열 또는 다른 대기열에서 비동기 실행하도록 스케줄링합니다. 모든 작업 항목의 실행이 끝나면 그룹은 completion handler를 실행합니다. Group의 모든 작업이 실행을 완료할 때까지 동기식으로 대기할 수도 있습니다.

- DispatchGroup은 async에서만 사용할 수 있습니다.
- async로 처리되는 작업은 시스템이 관리해주기 때문에 끝나는 시점을 예측할 수 없기 때문에 Group을 사용한다.
- sync는 작업이 끝나는 시점을 예측할 수 있으므로 작업 종료 시점을 따로 추적할 필요가 없다.

<br>

## DispatchGroup 사용하기

### DispatchGroup 초기화

init 메서드로 바로 초기화해 필요할 만큼 인스턴스를 만들어서 사용

### group에 등록 : enter, leave

Dispatch group에 등록하는 방법은 두가지가 있다.

- async 를 호출하면서 파라미터로 group을 지정
    
    ```swift
    let group = DispatchGroup()
    
    DispatchQueue.main.async(group: group) {}
    ```
    
- async 코드 앞뒤로 enter, leave 를 호출하여 group을 지정
    
    ```swift
    group.enter()
    DispatchQueue.main.async {}
    group.leave()
    ```
    

### 작업을 추적

notifiy : DispatchGroup의 업무 처리가 끝나는 시점에 원하는 동작을 수행하기 위한 메서드

notify의 파라미터 queue는 코드블록을 실행시킬 queue를 말합니다.

```swift
let work = DispatchWorkItem {
		sleep(2)
	  print("작업 처리 중...")
		sleep(2)
}

let group = DispatchGroup()

DispatchQueue.global().async(group: group)

group.notifiy(queue: .main) {
	print("모든 작업이 끝났습니다.")
}
```

### 작업 기다리기

wait : DispatchGroup의 수행이 끝나기를 기다리기만 하는 메서드입니다. notify와 달리 코드 블록을 실행하지 않습니다.

wait 메서드 안에 timeout 파라미터를 설정해줄 수 있습니다. timeout시간만큼 group의 작업이 끝나기를 기다리고 작업이 끝나지 않는다면 기다리지 않고 다음 코드를 실행한다.

```swift
let work = DispatchWorkItem {
		sleep(2)
	  print("작업 처리 중...")
		sleep(2)
}

let group = DispatchGroup()

DispatchQueue.global().async(group: group)

group.wait()
print("모든 작업이 끝났습니다.")
```
<br>

## Thread가 하나의 값에 동시에 접근하는 경우

하나의 값에 여러개의 스레드가 동시에 접근하면 Race Condition이 발생합니다.

그 이유는 Swift의 대부분의 타입은 Thread Safe하지 않기 때문입니다.

그 말은 여러 스레드에서 동시에 접근 가능한 상태라는 말입니다.

<br>

### DispatchSemaphore

counting semaphore를 이용하여 여러 execution context에서 리소스에 대한 액세스를 제어하는 개체입니다.

dispatch semaphore는 traditional counting semaphore를 효율적으로 구현한 것입니다. calling thread를 차단해야하는 경우에만 세마포어를 kernel로 호출합니다. calling thread가 차단할 필요가 없으면 kernel호출이 이루어지지 않습니다. signal() 메소드를 호출하여 semaphore count를 증가시키고 wait() 또는 타임아웃을 지정하는 변형 중 하나를 호출하여 semaphore count를 감소시킵니다.

- wait : semaphore count 감소
- signal : semaphore count 증가
- 반드시 wait 과 signal을 짝지어서 호출해야한다.

<br>

### Seial Queue를 활용하여 race condition 해결

질서없이 코드에 접근하여 문제가 발생한 것으로 순서를 만들어주면 해결된다.

따라서 Serial Queue를 이용하여 순서를 정해줄 수도 있다.
