# frame과 bounds

## frame이란?

superview 좌표계에서 view의 크기와 위치를 나타낸다.

예를 들어 버튼이라면, 버튼을 담고있는 superView내의 좌표에서 버튼은 어디에 위치하는 지 나타내는 것입니다.

### frame의 타입은 CGRect

frame의 타입은 CGRect입니다.

CGRect을 생성하는 init() 를 살펴보면 `init(origin: CGPoint, size: CGSize)` origin과 size가 있습니다.

- origin  
    슈퍼뷰의 원점을 0으로 두고 얼마나 떨어져있는지를 나타내는 좌표입니다.
    
- size  
    view영역을 모두 감싸는 사각형의 크기를 나타낸다.
    
이말 뜻은 view 자체의 크기가 아니라서 삼각형 형태라면 삼각형을 감싸는 사각형의 크기를 나타냅니다.
    
<br>

## Bounds란?

자신의 좌표계에게 위치와 크기를 나타낸다.  
이것은 superview와 관련없이 자기 자신의 좌표 사이즈를 나타내는 것이다.

### bounds의 타입도 CGRect

bounds도 CGRect 타입으로 origin, size를 가지고 있다. 하지만 frame과 다른만큼 origin, size도 다르다.

- origin  
    자신의 기준으로 하는 좌표이다.
    
- size  
    뷰 자체의 크기를 나타낸다. 이 값은 변하지 않는다
    
<br>

## frame과 bounds는 언제 사용할까?

나는 frame과 bounds의 차이를 scrollview를 통해 알게되었다.  
scrollview를 생성하면 

<img width="186" alt="스크린샷 2022-01-15 오후 6 11 25" src="https://user-images.githubusercontent.com/40068674/149617741-a63176b0-8d26-442a-a929-b55da58792a7.png">

두개가 나타난다.  
frame layout guide를 이용하여 superview와 제약을 걸 수 있는데

이때, 제약을 통해 size를 지정하게 되면 frame의 size는 고정이 된다.  
반면에 bounds의 size는 content의 size를 의미한다.  
코드에서는 `bounds.contentsize` 로 나타낼 수 있다.

### code로 나타내는 frame과 bounds 

- frame  
    - origin  
        `scrollView.frame.origin.x` , `scrollView.frame.origin.y` 
    - size   
        `scrollView.frame.size.width`, `scrollView.frame.size.height`
        
- bounds  
    - origin  
        `scrollView.bounds.origin.x` , `scrollView.bounds.origin.y` 또는  
        `scrollView.contentOffset.x`, `scrollView.contentOffset.y`
        
    - size  
        `scrollView.contentSize.width`,  `scrollView.contentSize.height`
        

content가 스크롤되어 origin, size가 변경되거나  
셀이 추가되어 content의 size가 변경되는 경우 frame과 bounds를 사용할 수 있다.
