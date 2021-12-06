# JSON Decoder

## JSON Decoder란?
JSON 객체로부터 데이터 타입의 인스턴스를 decode하는 객체이다.

<br/>

### JSON Decoding하는 방법
codable 프로토콜을 채택해서 JSONDecoder 인스턴스를 사용해 볼 수 있습니다.
```swift
struct PersonProduct: Codable {
    let name: String
    let age: Int
    let home: String
}

let singleJSON =
        """
           {
               "name":"Jerry",
               "age":26,
               "home":"Seoul"
            }
        """.data(using: .utf8)!

let decoder = JSONDecoder()
let product = try decoder.decode(PersonProduct.self, from: singleJSON)

print(product.name) // Prints "Jerry"
```
<br/>

### Assert파일에서 JSON 가져와서 Decoding

NSDataAsset 의 name을 이용해서 data를 가져온 후 decode

```swift
// Asset파일 AssetSingleJsonFile
{
		"name":"Jerry",
		"age":26,
		"home":"Seoul"
}

enum Decoder<Element: Codable> {
    
    static func decode(from jsonName: String) -> Element? {
        guard let data: NSDataAsset = NSDataAsset(name: jsonName) else {
            return nil
        }
        
        let decoder = JSONDecoder()
        let products = try? decoder.decode(Element.self, from: data.data)
        
        return products
    }
}

let decodedResult = Decoder<PersonProduct>.decodeAssetJsonFile(from: "AssetSingleJsonFile")
print(decodedResult.name)   // Prints "Jerry"
```

<br/>

### JSON array를 Decoding하는 방법

```swift
let json = """
{
    "name": "Durian",
    "points": 600,
    "description": "A fruit with a distinctive scent."
},
{
    "name": "Apple",
    "points": 300,
    "description": "Eating apples has a 90% chance of dying within 100 years."
},
{
    "name": "Banana",
    "points": 100,
    "description": "If you step on a banana, it's slippery."
}
""".data(using: .utf8)!
```

배열로된 JSON 파일을 decoding 하는 방법이다.

```swift
// 배열 객체를 직접 만들어줌
struct GroceryProduct: Codable {
    var name: String
    var points: Int
    var description: String?
}

let decoder = JSONDecoder()
let product = try decoder.decode([GroceryProduct].self, from: json)

print(product.first.name) // Prints "Durian"

// 메서드를 통해 전달
enum Parser<Element: Decodable> {
    static func decode() -> Element? {
        guard let data: NSDataAsset = NSDataAsset(name: jsonname) else {
            return nil
        }
        
        let decoder = JSONDecoder()
        let products = try? decoder.decode(Element.self, from: data.data)
        
        return products
    }
}

let decodedResult = Parser<[GroceryProduct]>.decode()
print(decodedResult.first.name)   // Prints "Durian"
```

<br/>

### JSON File에 키가 반환되지 않는 경우

 JSON File에 data가 없고 Product에 정의하고 싶다면 optional을 사용하면 된다.

```swift
struct PersonProduct: Codable {
    let name: String
    let age: Int
    let home: String
    
    let image: String?
}

// image 는 nil값으로 반환된다.
```

<br/>

### 중첩 JSON 구조

중첩된 구조는 타입을 맞추어 설계해야한다.

```swift
// Asset파일 NestedSingleJsonFile
{
    "name":"Jerry",
    "age":26,
    "home":{"gu":"Songpa"}
}

// product
struct PersonProduct: Codable {
    let name: String
    let age: Int
    let home: Address
    
    let image: String?
}

struct Address: Codable {
    let gu: String
}

// test code
let result = Decoder<PersonProduct>.decodeAssetJsonFile(from: .nestedJsonFile)
        
XCTAssertEqual(result?.home.gu, "Songpa")
```

<br/>

### 결론

JSON 파일을 decoding하는 로직은 디버깅이 어렵다😢  
LLDB 공부하자!
