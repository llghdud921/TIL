# App의 Interface orientation

<br>

## AppDelegate를 이용한 interface orientation 설정

appdelegate에서 application(_:supportedInterfaceOrientationsFor:) 메소드를 사용해볼 수 있다.


### **application(_:supportedInterfaceOrientationsFor:)란?**

지정된 창의 뷰 컨트롤러에 사용할 인터페이스 방향을 딜러에게 요청합니다.

```swift
optional func application(_ application: UIApplication, 
supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask
```


### parameter

- application  
    singleton 앱 객체
    
- window  
    interface orientation을 검색하는 window

### return 값
viewController에 사용할 방향을 나타내는 UIInterfaceOrientationMask 상수의 bit mask입니다.

<br>

### 설명

이 메서드는 앱에서 지원되는 총 interface orientation 집합을 반환합니다. 

특정 viewController를 회전할지 여부를 결정할 때 이 방법에 의해 반환되는 방향은 루트 viewController 또는 맨 위에 표시되는 viewController가 지원하는 방향과 교차합니다. **회전이 허용되기 전에 앱과 뷰 컨트롤러가 일치해야 합니다.**

이 방법을 구현하지 않으면 앱은 앱의 Info.plist에 있는 UIInterfaceOrientation 키의 값을 기본 인터페이스 방향으로 사용합니다. 

- 앱의 info.plist가 default이고
- 우선순위는 해당 메소드가 높다

<br>


### 예제 코드

```swift
func application(_ application: UIApplication, supportedInterfaceOrientationsFor window: UIWindow?) -> UIInterfaceOrientationMask {
    // 가로 세로 둘다 지원
    return [.portrait, .landscape]
        
    // 가로만 지원
    return [.landscapeLeft, .landscapeRight]
        
    // 세로만 지원
    return [.portrait]
}
```
<br>

## ViewController 내부에서 supportedInterfaceOrientations property override해서 사용

## supportedInterfaceOrientations란?

뷰 컨트롤러가 지원하는 인터페이스 방향

이 속성은 뷰 컨트롤러가 지원하는 방향을 지정하는 bitMask를 반환합니다. 

장치 방향이 변경되면 시스템은 root View Controller 또는 창을 채우는 modal view controller에서 이 메서드를 호출합니다. View Controller가 새로운 방향을 지원하는 경우 시스템은 윈도우와 뷰 컨트롤러를 회전시킵니다. 시스템은 뷰 컨트롤러의 shouldAutorate 메서드가 true를 반환하는 경우에만 이 메서드를 호출합니다.

뷰 컨트롤러가 지원하는 방향을 선언하려면 이 방법을 재정의합니다. 기본값은 iPad idiom은 all 이고 iPhone idiom는 allButUpsideDown입니다. 반환되는 값은 0이 아니어야 합니다.

회전 여부를 결정하기 위해 시스템은 Info.plist 파일 또는 AppDelegate의 application(_:supportedInterfaceOrientationsFor:)에 의해 결정된 View Controller의 지원되는 방향과 앱의 지원되는 방향을 비교합니다.

<br>


### supportedInterfaceOrientations 사용 방법

1. navigationController class를 생성하고 
    
    supportedInterfaceOrientations를 override하여 navigationController의 현재 가장 최상위 ViewController를 반환하게 구현한다
    
    ```swift
    override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
    	self.topViewController as? ExpoInformationViewController
    }
    ```
    

1. 각 ViewController에 supportedInterfaceOrientations를 override하여 원하는 속성값을 return
    
    ```swift
    override var supportedInterfaceOrientations: UIInterfaceOrientationMask {
    	return [.portrait]
    }
    ```

<br>
    

[참고]

[https://velog.io/@wonhee010/특정-ViewController에서-화면-회전-처리](https://velog.io/@wonhee010/%ED%8A%B9%EC%A0%95-ViewController%EC%97%90%EC%84%9C-%ED%99%94%EB%A9%B4-%ED%9A%8C%EC%A0%84-%EC%B2%98%EB%A6%AC)
