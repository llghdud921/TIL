# NSDataAssetName extension을 이용한 하드코딩 방지

<br/>

기존에 string으로 매개변수를 전달하는 방식을 이용했는데 이 방식은 하드코딩으로 짜여졌고 실수를 유발할 것같아서 코드를 수정하고자 하였습니다.

```swift
// Parser Code
enum Parser<Element: Decodable> {
    static func decode(from jsonName: String) -> Element? {
        guard let data = NSDataAsset(name: jsonName) else {
            return nil
        }
        
        let products = try? JSONDecoder().decode(Element.self, from: data.data)
        
        return products
    }
}

// 실행 코드
Parser<ExpoInformation>.decode(from: "Single_JSON_File")
```

<br/>

### NSDataAsset 내 매개변수 타입

NSDataAsset내 매개변수 타입을 보니 NSDataAssetName입니다.

`NSDataAsset(name:**NSDataAssetName**)`

찾아가보니 홀랭~ 그냥 string이군여 하지만 swift에서 이름도 지어줬으니 그대로 extension 할겁니다요

![스크린샷 2021-12-08 오전 2 06 00](https://user-images.githubusercontent.com/40068674/145075202-a39119cd-5ae4-443f-8cfc-d66547364bf7.png)

<br/>

### extension을 이용해서 name 생성

파일명이 swift 방식이랑 맞지 않는 경우에도 이방법을 사용하면 훨씬 보기 좋을것같아요

```swift
// 1. NSDataAssetName 정의
extension NSDataAssetName {
    static let singleJSONFile: NSDataAssetName = "Single_JSON_File"
}

// 2. string이므로 String으로 정의도 가능하다
extension NSDataAssetName {
    static let singleJSONFile = "Single_JSON_File"
}

// 3. NSDataAssetName 생성
extension NSDataAssetName {
    static let singleJSONFile = NSDataAssetName("Single_JSON_File")
}
```

<br/>

실행코드를 살펴보면 아주 깔꼼합니다.

```swift
Parser<ExpoInformation>.decode(from: .singleJSONFile)
```
