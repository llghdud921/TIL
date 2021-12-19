# 동시성 프로그래밍 Concurrency Programing (1)

<br>

## 동기와 비동기

- 동기 sync : 작업이 끝나기를 기다리는 것
- 비동기 async :  작업이 끝나기를 기다리지 않고 다음 코드 블럭을 실행시킨다.

둘의 차이는 실행 종료 시점을 알 수 있는 가 이다 

<br>

## 동시성 프로그래밍이란?

하나의 CPU가 여러 작업을 동시에 처리하는 것입니다.

동시성 프로그래밍은 여러 개의 스레드를 이용하여 동시에 여러 작업을 처리합니다.

- context switch를 하면서

### 동시성 프로그래밍을 사용하는 이유?

- 성능 최적화
    
    동시성 프로그래밍을 여러 개의 스레드를 이용해서 일을 나누어 효율적인 작업 처리를 할 수 있게 한다. 해당 처리는 성능을 개선하여 최적화로 이어진다.
    
- 사용성
    
    동시성 프로그래밍을 통해 성능이 좋아짐으로써 사용자의 사용성, 반응성 또한 좋아지게 된다.
    
<br>

## Swift 에서 동시성 프로그래밍을 사용하는 방법

애플에서는 코드로서 동기와 비동기 처리만 하면 알아서 스레드 관리해주는 방식인 GCD를 제공한다.


### GCD란 무엇일까?

Grand Central Dispatch 로 멀티 코어 환경화 멀티 스레드에 환경에서 최적화된 프로그래밍을 할 수 있도록 애플이 개발한 기술이다.

GCD를 사용하기 위해서 Dispatch Queue class를 주로 사용하게 된다.

<br>

### Dispatch Queue란?

GCD를 사용하기 위한 대기열로 이 대기열에 작업을 추가해주기만 하면 시스템은 알아서 스레드를 관리하여 작업을 처리한다.

여기서 dispatch queue에 작업을 넘길 때 2가지를 정해준다.

- 단일 스레드 vs 다중 스레드
- 동기 vs 비동기

<br>

### Dispatch Queue : 단일 스레드 vs 다중 스레드

위에서 말했 던 단일 스레드 vs 다중 스레드를 GCD에서는 Serial, Concurrent로 구분할 수 있다.

- Serial Queue
    
    `DispatchQueue.main` 으로 사용
    
    단일 스레드로 작업을 처리
    
- Concurrent Queue
    
    `DispatchQueue.global()` 로 사용
    
    다중 스레드에서 작업을 처리
    

DispatchQueue를 초기화할 때, attributes를 .concurrent로 설정하지 않으면 기본값은 serial이다.

<br>

### main thread란?

main thread는 앱의 기본이 되는 스레드입니다. 이 메인 스레드는 앱의 생명주기와 같은 생명주기를 가지는, 앱이 실행되는 동안에는 늘 메모리에 올라와있는 기본 스레드입니다.

- 전역적으로 사용이 가능합니다.
- global 스레드들과는 다르게 Run Loop가 자동으로 설정되고 실행됩니다. 메인 스레드에서 동작하는 Run Loop를 Main Run Loop라고 합니다.
- UI 작업은 메인 스레드에서만 작업할 수 있습니다.

<br>

### Dispatch Queue : 동기 vs 비동기

동기, 비동기는 작업이 끝나기를 기다리는 것을 정한다.

main, global 에서는 동기와 비동기를 결정해주어야한다.

- DispatchQueue.main.sync
    
    main thread 에 원래 있던 작업과 sync에 작성한 코드블럭이 서로 작업이 끝나길 기다리고 있기 때문에 교착상태에 빠진다.
    
    따라서 main.sync 는 런타임시 에러가 나므로 사용을 지양해야한다.
    
    하지만 사용 가능하기도 하다.
    
    global.async 에서 main.sync를 호출한다면 가능하다. 
    
- DispatchQueue.main.async
    
    main 스레드에 비동기로 일을 처리하는 코드.
    
    main thread는 단일 스레드이므로 비동기여도 동시에 작업이 처리되지 못하고 순서대로 처리된다.
    
- DispatchQueue.main.sync
    
    main 스레드가 아닌 다른 스레드를 만들어서 작업을 처리하지만 동기적으로 작업을 처리하기 때문에 각 작업이 끝나기를 기다린다. 따라서 순서대로 작업이 처리된다.
    
- DispatchQueue.main.async
    
    main 스레드가 아닌 다른 스레드를 만들어서 비동기적으로 처리한다.
    
    어떤 코드가 먼저 실행되는지 예측할 수 없다.
