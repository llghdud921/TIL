# 동시성 프로그래밍 Concurrency Programing (2)

## DispatchQueue의 초기화

DispatchQueue는 serial, concurrent 외에 커스텀하여 사용할 수 있습니다.

```swift
DispatchQueue(label: String,
              qos: DispatchQoS = .unspecified,
						  attributes: DispatchQueue.Attributes = [],
						  autoreleaseFrequency: DispatchQueue.AutoreleaseFrequency = .inherit,
						  target: DispatchQueue? = nil)
```

DispatchQueue를 초기화하는 메서드의 파라미터를 살펴보자.

### label
DispatchQueue의 label은 디버깅 환경에서 추적하기 위해 작성하는 String 값

### qos
Quality Of Service, 실행 될 task의 우선 순위를 정해주는 값

### attributes
DispatchQueue의 속성값.

- `.concurrent` :  다중 스레드 환경에서 처리하는 DispatchQueue
- 빈배열(기본값) or `.Serial` : 단일 스레드 환경에서 처리하는 DispatchQueue
- `.initiallyInactive` : sync와 async에 담긴 코드블럭의 처리를 제어할 수 있음
    
     sync나 async를 호출해도 작업을 Queue에 담고, active()를 호출하기 전까지 작업을 처리하지 않음
    

### **autoreleaseFrequency**
자동으로 객체를 해제하는 빈도의 값을 결정하는 파라미터.

기본값은 inherit이다.

- inherit : target과 같은 빈도를 가짐
- workItem : workItem이 실행될 때마다 객체들을 해제함
- never : autorelease 하지 않음

### target
코드 블록을 실행할 큐를 target으로 설정

<br>

## Qos
QoS Class는 DispatchQueue에서 수행할 작업을 분류합니다. task의 qulify을 지정하여 앱에 중요도를 표시합니다.  

task를 예약할 때 시스템은 더 높은 서비스 클래스를 가진 task의 우선 순위를 지정합니다.

우선 순위가 높은 task은 우선 순위가 낮은 task보다 더 빠르고 더 많은 리소스로 수행되기 때문에 일반적으로 우선 순위가 낮은 task보다 더 많은 에너지가 필요합니다.   

앱이 수행하는 task에 대해 적절한 QoS 클래스를 정확하게 지정하면 앱이 응답하고 에너지 효율적임을 보장합니다.  

- User Interactive  
    사용자 인터페이스 새로고침, 애니메이션 등 사용자와 상호작용하는 작업에 할당.
    작업이 빠르게 수행되지 않으면,UI는 멈춘다.
    
- User-initiated  
    문서를 열거나 버튼을 클릭해 액션을 수행하는 것처럼 빠른 결과를 요구하는 상호작용 작업에 할당
    
- Default  
    기본값이다. Use initiated 와 Utility 사이
    
- Utility  
    데이터를 읽거나 다운로드 하는 작업처럼 작업이 시간이 걸리거나 즉각적인 결과가 요구되지 않는 작업에 할당한다.
    
- BackGround  
    Index 생성, 동기화, 백업 등 사용자가 볼 수 없는 백그라운드 작업에 할당
    
- Unspecfied  
    Unspecified는 qos 정보가 없음을 나타내며 시스템이 qos를 추론해야한다.
        
<br>

## async의 Parameter
코드 블럭을 비동기적으로 실행하도록 예약하고 선택적으로 디스패치 그룹과 연결- 

```swift
func async(group: DispatchGroup? = nil, 
					 qos: DispatchQoS = .unspecified, 
					 flags: DispatchWorkItemFlags = [], 
				   execute work: @escaping () -> Void)
```

- group
    여러 스레드에서 비동기 처리를 하면 여러개의 작업을 함께 관리해야한다. 이때 작업 항목과 연결하는 Dispatch groupd이다.  nil을 지정하면 코드 블럭이 그룹과 연결되지 않습니다.
    
- qos
    위에 적힌 qos와 동일. 작업의 우선순위를 지정
    
- flags
    코드 블록을 실행할 때 적용할 추가 속성입니다.  
    기본값은 아무 속성도 부여하지 않습니다. 
    
    - assingCurrentContext  
        현재 실행 컨텍스트의 특성과 일치하도록 작업 항목의 속성을 설정합니다.
        
    - barrier   
        동시 대기열에 제출될 때 작업 항목이 차단 역할을 하도록 합니다.
        
    - detached    
        작업 항목의 속성을 현재 실행 컨텍스트에서 분리합니다.
        
    - enforceQos    
        블록과 관련된 qos를 선호합니다.
        
    - inheritQoS  
        현재 실행 컨텍스트와 연결된 qos를 선호합니다.
        
    - noQoS  
        qos를 할당하지 않고 작업 항목을 실행합니다.
        
 <br>
