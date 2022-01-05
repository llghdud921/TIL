# URLProtocol을 이용한 NetworkTest

### 개요

네트워크 테스트를 하기 위해서 아래의 방법으로 구현하였습니다.

1. `URLSession`이 네트워킹을 받아서 처리하는 `DummyData` 를 구현
2. `URLSession`을 대체할 객체로 `StubURLSession` 을 만들어  `DummyData`를 `URLSessionDataTask`에 전달
3. `URLSessionDataTask` 를 대체할 객체로  `StubURLSessionDataTask` 를 만들어 `resume()`을 통해`completionHandler`를 수행 

참고 : [https://yagom.net/courses/unit-test-작성하기/lessons/테스트를-위한-객체-만들기/topic/테스트를-위한-객체를-이용해서-테스트-작성하기/](https://yagom.net/courses/unit-test-%ec%9e%91%ec%84%b1%ed%95%98%ea%b8%b0/lessons/%ed%85%8c%ec%8a%a4%ed%8a%b8%eb%a5%bc-%ec%9c%84%ed%95%9c-%ea%b0%9d%ec%b2%b4-%eb%a7%8c%eb%93%a4%ea%b8%b0/topic/%ed%85%8c%ec%8a%a4%ed%8a%b8%eb%a5%bc-%ec%9c%84%ed%95%9c-%ea%b0%9d%ec%b2%b4%eb%a5%bc-%ec%9d%b4%ec%9a%a9%ed%95%b4%ec%84%9c-%ed%85%8c%ec%8a%a4%ed%8a%b8-%ec%9e%91%ec%84%b1%ed%95%98%ea%b8%b0/)

하지만 URLSessionDataTask를 생성하는 과정에서 deprecated 경고가 발생하였고 이를 개선하기 위해서 URLProtocol을 이용한 NetworkTest 방식으로 재구성하였습니다.

[WWDC18 Testing Tips & Tricks](https://developer.apple.com/videos/play/wwdc2018/417/)  을 참고 하였으며 기존에 URLSession을 stub으로 작성해주었다면 해당 방식은 MockProtocol을 가진 URLSession을 configuration 하는 것입니다.

<br>

## URLProtocol은 무엇일까?

프로토콜별 URL data load를 처리하는 추상 클래스입니다.  
URL Loading을 위한 확장성 지점을 제공하는 하위 클래스로 설계되었다고 합니다. 

WWDC에서 

> Foundation은 HTTPS와 같은 일반적인 프로토콜에 대한 기본 제공 프로토콜 하위 클래스를 제공합니다. 그러나 우리는 테스트에서 이러한 프로토콜을 재정의할 수 있습니다.
> 

이런 설명이 있는데 HTTP도 프로토콜이자나여 URLProtocol도 Http와 같은 프로토콜이며  
이를 통해 Networking에 필요한 사용자 정의 프로토콜을 만들수 있다~ 이런 말인 것 같습니다

그래서 Http가 아닌 MockProtocol을 통해서 테스트하는 경우 아래와 같이 코드 길이가 차이가 납니다  
무엇보다도 `URLSessionDataTask` 가 없기 때문에 관련 경고가 없어졌습니다ㅎㅎ

- MockProtocol 사용 안하는 경우
    
    ```swift
    // URLSessionProtocol 프로토콜 정의
    protocol URLSessionProtocol {
        func dataTask(with url: URL,  completionHandler: @escaping DataTaskCompletionHandler) -> URLSessionDataTask
    }
    
    // URLSession이 URLSessionProtocol 프로토콜 채택
    extension URLSession: URLSessionProtocol { }
    
    // URLSessionProtocol을 채택한 StubURLSession 구현
    class StubURLSession: URLSessionProtocol {
        var dummyData: DummyData?
    
        init(dummy: DummyData) {
            self.dummyData = dummy
        }
    
        func dataTask(with url: URL, completionHandler: @escaping DataTaskCompletionHandler) -> URLSessionDataTask {
            return StubURLSessionDataTask(dummy: dummyData, completionHandler: completionHandler)
        }
    }
    
    // Session을 주입하는 코드에 URLSessionProtocol 타입 적용 
    
    // StubURLSessionDataTask도 만들어야함........ 등등등
    ```
    
- MockProtocol 사용하는 경우
    
    ```swift
    // MockURLProtocol을 가진 세션 객체 생성
    class MockSession {
        static var session: URLSession {
            let configuration = URLSessionConfiguration.ephemeral
            configuration.protocolClasses = [MockURLProtocol.self]
            return URLSession(configuration: configuration)
        }
    }
    ```
    
    <br>

## URLProtocol를 사용하는 방법

<img src="https://user-images.githubusercontent.com/40068674/148263357-35948f22-148b-404d-a7b2-7878b79431df.png" width="600" >

URLSessionConfiguration안에 있는 MockProtocol은 URLProtocolClient 프로토콜을 통해 피드백을 시스템에 전달합니다.  
위 그림을 통해 MockProtocol이 어떤 식으로 동작하는 지 감을 잡고 내부 메서드를 통해 정확한 동작을 알아봅시다

### **subclasses 시에 필수 메서드**

- **canInit(with:)**  
    프로토콜 subclass가 지정된 request을 처리할 수 있는지 여부를 결정합니다.  
    프로토콜 subclass가 요청을 처리할 수 있으면 true이고 그렇지 않으면 false입니다.  
    canInit()을 재정의하여 시스템이 우리에게 제공하는 모든 request에 관심이 있음을 나타낸다고 합니다.
    
- **canonicalRequest(for:)**  
    지정한 요청의 표준 버전을 반환합니다.  
    canonical이 표준? 정식 버전? 을 뜻하는데 사실 이게 뭔지 모르겠습니다.. 그냥 반환합니다
    
- **startLoading()**  
    request의 프로토콜별 load를 시작합니다.  
    이 메서드가 호출되면 subclass 구현체가 request을 load하기 시작하여 URLProtocolClient 프로토콜을 통해 URL loading system에 피드백을 제공해야 합니다.  
    대부분의 작업이 startloading, endLoading에서 발생한다고 합니다.
    
- **stopLoading()**  
    request의 프로토콜별 load를 중지합니다.  
    이 메서드가 호출되면 subclass 구현체가 request load를 중지해야 합니다. 이것은 취소 작업에 대한 응답일 수 있으므로, 프로토콜 구현은 로드가 진행되는 동안 이 호출을 처리할 수 있어야 합니다. 프로토콜이 이 메서드에 대한 호출을 받으면 클라이언트에 대한 알림 전송도 중지해야 합니다.
    
<br>

### MockURLProcotol 작성

```swift
// URLProtocol 상속받은 MockURLProcotol
class MockURLProtocol: URLProtocol {

    // error, data, and response 를 담은 객체
    static var mockURLs = [URL?: (error: Error?, data: Data?, response: HTTPURLResponse?)]()
   
    // 지정된 request을 처리할 수 있는지 여부를 결정
    override class func canInit(with request: URLRequest) -> Bool {
        return true
    }
    
    // 지정한 request의 표준 버전을 반환
    override class func canonicalRequest(for request: URLRequest) -> URLRequest {
        return request
    }
    
    override func startLoading() {
        if let url = request.url {
            if let (error, data, response) = MockURLProtocol.mockURLs[url] {
                
                // mock response을 client로 전송
                if let responseStrong = response {
                    self.client?.urlProtocol(self, didReceive: responseStrong, cacheStoragePolicy: .notAllowed)
                }
                
                // mock data을 client로 전송
                if let dataStrong = data {
                    self.client?.urlProtocol(self, didLoad: dataStrong)
                }
                
                // mock error을 client로 전송
                if let errorStrong = error {
                    self.client?.urlProtocol(self, didFailWithError: errorStrong)
                }
            }
        }
        
        // 프로토콜 구현이 로드되었음을 client로 전송
        self.client?.urlProtocolDidFinishLoading(self)
    }
    
    override func stopLoading() {
        // request가 취소되거나 완료되었을 때 호출
    }
}
```

<br>

### URLSessionConfiguration이 어떻게 URLProtocol을 담고 있는지

```swift
static var session: URLSession {
    let configuration = URLSessionConfiguration.ephemeral
    configuration.protocolClasses = [MockURLProtocol.self]
    return URLSession(configuration: configuration)
}
```

- **URLSessionConfiguration.ephemeral**  
    URLProtocol을 담는 configuration은 emphemeral(임시)인데 왜 emphemeral인지는 모르겟습니당..
    
- **protocolClasses**  
    세션에서 request를 처리하는 추가 프로토콜 subclass 배열로 하나 이상 사용자 지정 프로토콜이 있는 세션에서 사용할 수 있는 common networking protocols set를 확장하려면 이 배열을 사용합니다.
    

위의 만들어진 Mock객체를 가지고 테스트 코드를 작성하면 완성~
