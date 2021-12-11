# awakeFromNib

<br/>

### awakeFromNib()란?

Interface Builder 아카이브 또는 nib 파일에서 로드된 후 서비스를 위해 receiver를 준비

<br/>

### awakeFromNib에 대한 설명

nib-loading 구조는 nib archive에서 재생성된 각 오브젝트에 awakeFromNib 메시지를 보내지만 archive의 모든 오브젝트가 로드되고 초기화된 후에만 전송됩니다. 오브젝트가 awakeFromNib 메시지를 수신하면 모든 Outlet 과 액션 연결이 이미 설정되어 있어야 합니다.

부모 클래스에 필요한 추가 초기화를 수행할 수 있는 기회를 제공하려면 `awakeFromNib`의 `super` 구현을 호출해야 합니다. 이 메서드의 기본 구현은 아무 것도 수행하지 않지만 많은 UIKit 클래스는 비어 있지 않은 구현을 제공합니다. 자신의 awakeFromNib 메서드 중에 언제든지 `super` 구현을 호출할 수 있습니다.

인스턴스화 프로세스 동안 archive의 unarchived되고 타입에 맞는 메소드로 초기화됩니다. NSCoding 프로토콜(UIView 및 UIViewController의 모든 하위 클래스 포함)을 준수하는 개체는 `initWithCoder`: 메서드를 사용하여 초기화됩니다. NSCoding 프로토콜을 준수하지 않는 모든 개체는 `init` 메소드 방법을 사용하여 초기화됩니다. 객체가 인스턴스화되고 초기화되면 nib-loading 코드가 모든 개체에 대한 `outlet` 및 `action` 연결을 다시 설정합니다. 그런 다음 객체의 `awakeFromNib` 메소드를 호출합니다. 니브 로드 프로세스 중에 이어지는 단계에 대한 자세한 내용은 [리소스 프로그래밍 안내서의 니브 파일](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/LoadingResources/CocoaNibs/CocoaNibs.html#//apple_ref/doc/uid/10000051i-CH4-SW8)을 참조하십시오.

일반적으로 설계 시 수행할 수 없는 추가 설정이 필요한 개체에 대해 awakeFromNib을 구현합니다. 예를 들어, 이 방법을 사용하여 컨트롤의 기본 구성을 사용자 선호 설정과 일치하도록 customize하거나 다른 컨트롤의 값을 지정할 수 있습니다. 또한 개별 컨트롤을 프로그램의 이전 상태로 복원하는 데 사용할 수도 있습니다.

<br/>

### awakeFromNib가 호출되는 인스턴스화 프로세스 정리

위의 설명을 보고 인스턴스화 프로세스를 다시 정리하자면 

1. 클래스 아카이브는 unarchived되고 
2. 타입에 맞는 메소드로 초기화 과정을 거칩니다.
    - **NSCoding Protocol 준수 객체(UIView 및 UIViewController의 모든 하위 클래스)**
        
        `init(Coder:)` 메소드로 초기화
        
    - 그 외 NSCoding 프로토콜을 준수하지 않는 모든 객체
        
        `init()` 메소드로 초기화
        
3. 객체가 인스턴스화되고 초기화되면 nib-loading 코드가 모든 개체에 대한 `outlet` 및 `action` 연결을 다시 설정
4. 객체의 `awakeFromNib` 호출

<br/>

### awakFromNib 언제 사용할까?

xib나 storyBoard를 통해서 View 인스턴스를 생성하는 경우 초기화 과정에서 unarchived되어 초기화 시점에는 View 인스턴스를 호출할 수 없습니다.

그래서 View의 추가 설정이 필요하면 `outlet` 이나 `action` 연결이 완료된 시점인 `awakeFromNib` 시점에서 설정 코드를 구현합니다.

이 방법으로 컨트롤의 기본 구성을 사용자 선호 설정과 일치하도록 customize하거나 다른 컨트롤의 값을 지정할 수 있습니다.

<br/>

참고 URL 
[https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)
