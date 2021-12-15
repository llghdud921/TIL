# NSMutableAttributedString을 이용해서 String의 특정 부분만 속성 변경하기

String의 일부만 속성을 변경하기 위해서 NSMutableAttributedString을 사용합니다.

<br/>

## NSMutableAttributedString란?

텍스트 일부에 대한 관련 속성(예: visual style, hyperlink 또는 accessibility data)을 변경 가능한 문자열입니다.

<br/>

## NSMutableAttributedString 사용 방법

NSMutableAttributedString은 NSAttribedString에 추가하는 두가지의 메소드로 방법이 나누어져 있다.

<br/>

### 일치하는 부분 문자열 변경

1. NSMutableAttributedString에 적용할 text를 가져온다. 
2. text를 가지고 NSMutableAttributedString 객체를 생성한다.
    
    `let attributedString = NSMutableAttributedString(string: text)`
    
3. 바꾸는 범위와 바뀔 style로 attribute를 추가한다.
    
    `attributedString.addAttribute(.font, value: , range: )`
    
4. 속성이 적용된 NSMutableAttributedString을 View에 넣는다.
    
    `replaceLabel.attributedText = attributedString`

<br/>

전체 코드

```swift
guard let text = replaceLabel.text else {
    return
}
        
let attributedString = NSMutableAttributedString(string: text)
        
attributedString.addAttribute(.font, value: UIFont.preferredFont(forTextStyle: .title1, compatibleWith: UITraitCollection(legibilityWeight: .bold)),range:  (text as NSString).range(of: "NSMutableAttributedString"))
        
replaceLabel.attributedText = attributedString
```
<br/>

이 부분은 아쉽게도 일치하는 부분 문자열 1개만 수정한다.

replace를 이용해서 바꾸는 방식도 생각했는데 잘 구현이 되지 않아서 포기했다... 다음에 다시 시도.......

<br/>

### 문자열을 추가하는 방식으로 부분 문자열 변경

1. NSMutableAttributedString에 적용할 text를 가져온다. 
2. text를 가지고 NSMutableAttributedString 객체를 생성한다.
    
    `let attributedString = NSMutableAttributedString(string: text)`
    
3. 변경할 속성이 담긴 attribute를 생성한다.
    
    `let attributes: [NSAttributedString.Key: Any] = [.font: UIFont.preferredFont(forTextStyle: .title1)]`
            
    
4. attribute와 적용하는 text (부분 문자열) 을 가지고 NSAttributedString 생성한다.
    
    `NSAttributedString(string: "set", attributes: attributes)`
    
5. 생성된 NSAttributedString을 NSMutableAttributedString에 append
6. 속성이 적용된 NSMutableAttributedString을 View에 넣는다.
    
    `replaceLabel.attributedText = attributedString`
    
<br/>

전체 코드

```swift
guard let text = setLabel.text else {
    return
}
let attributeString = NSMutableAttributedString(string: text)
        
let attributes: [NSAttributedString.Key: Any] = [.font: UIFont.preferredFont(forTextStyle: .title1)]
        
attributeString.append()
        
setLabel.attributedText = attributeString
```

<br/>

### **NSAttributedString.Key**

속성의 key가 하기 이미지보다 더 많이 있다.

위에서 font 적용뿐만아니라 backgroundColer, attachment? 등 활용할 수 있다.

![스크린샷 2021-12-15 오후 12 32 32](https://user-images.githubusercontent.com/40068674/146125699-82ca6775-096a-4168-8c57-6451cb258825.png)

