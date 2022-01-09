# multipart/form

Http와 전송할 때 `Http request body`에 파일을 담아 전송을 한다.

이때 데이터타입을 `content-type`으로 명시할 수 있으며 해당 필드에 `MIME` 타입을 기술해줄 수 있는데 여러 타입 중 하나다 `multipart/form`이다. 

<br>

## multipart/form 작성하는 방법

1. URLRequest를 생성한다. (POST만 가능)  
    request에 setValue를 이용해서 content-type을 지정한다.    
    이때 boundary로 정할 String값을 생성하여 multipart 파라미터에 boundary를 포함한다.
    
    ```swift
    var baseBoundary: String {
        return "Boundary-\(UUID().uuidString)"
    }
    
    var request = URLRequest(url: url)
            
    request.httpMethod = "POST"
    request.setValue("multipart/form-data; boundary=\(boundary)",
                             forHTTPHeaderField: "Content-Type")
    ```
    <br>
    
2. multipart body를 구성한다.  
    필드 하나당 body에 boundary를 추가하여 파일을 구분한다. 여기서 주의할 점은 모든 문자열 뒤에 개행을 위해 \r\n이 추가된다.  
    하지만 body내에 포함되어있는 data를 분리하는 것은 \r\n이 두개  
    - boundary
    - name: value name
    - data : 해당 data
    
    ```swift
    var fieldDataString = "--\(boundary)\r\n"
    fieldDataString += "Content-Disposition: form-data; name=\"\(name)\"\r\n\r\n"
    fieldDataString += "\(data)\r\n"
    ```
    
    <br>
    
3. (추가) 파일 데이터 (이미지)를 포함한 body 구성한다.   
    이미지 데이터를 포함한 body를 구성하고 싶다면   
    - boundary
    - filename : image filename
    - content-type
    - fileData
    
    ```swift
    var fileDataString = "--\(boundary)\r\n"
    fileDataString += "Content-Disposition: form-data; name=\"\(fieldName)\"; filename=\"\(fileName)\"\r\n"
    fileDataString += "Content-Type: \(mimeType)\r\n\r\n"
    fileDataString += "\(fileData)\r\n"
    ```
    
    <br>
    
4. 마지막 boundary 추가  
    모든 파일을 넣은 Data에 마지막으로 아래 boundary string을 추가로 append해야한다.   
    다른 boundary string과 다르다. 끝에 `—` 가 추가됨  
    이것은 body의 끝을 알리게 된다.
    
    ```swift
    bodyString +=  "--\(boundary)--"
    ```
    
    <br>

1. (추가) Data extension  
    request에 구성한 string 값을 넣기 위해 UTF-8로 변환하여 Data 형식으로 append해야한다.  
    이때 Data를 extension해서 코드를 개선해볼 수 있다.
    
    ```swift
    extension Data {
        mutating func append(_ string: String) {
            guard let data = string.data(using: .utf8) else {
                return
            }
            self.append(data)
        }
    }
    ```
    
<br>

## multipart/form 구성 예

```swift
## HTTP Request Data

POST /file/upload HTTP/1.1[\r][\n]

Content-Length: 344[\r][\n]

Content-Type: multipart/form-data; boundary=Uee--r1_eDOWu7FpA0LJdLwCMLJQapQGu[\r][\n]

Host: localhost:8080[\r][\n]

Connection: Keep-Alive[\r][\n]

User-Agent: Apache-HttpClient/4.3.4 (java 1.5)[\r][\n]

Accept-Encoding: gzip,deflate[\r][\n]

[\r][\n]

--Uee--r1_eDOWu7FpA0LJdLwCMLJQapQGu[\r][\n]

Content-Disposition: form-data; name=files; filename=test.txt[\r][\n]

Content-Type: application/octet-stream[\r][\n]

[\r][\n]

aaaa

[\r][\n]

--Uee--r1_eDOWu7FpA0LJdLwCMLJQapQGu[\r][\n]

Content-Disposition: form-data; name=files; filename=test1.txt[\r][\n]

Content-Type: application/octet-stream[\r][\n]

[\r][\n]

1111

[\r][\n]

--Uee--r1_eDOWu7FpA0LJdLwCMLJQapQGu--[\r][\n]

## HTTP Response Data

HTTPHTTP/1.1 200 OK[\r][\n]

Server: Apache-Coyote/1.1[\r][\n]

Accept-Charset: big5, big5-hkscs, euc-jp, euc-kr...[\r][\n]

Content-Type: text/html;charset=UTF-8[\r][\n]

Content-Length: 7[\r][\n]

Date: Mon, 30 Jun 2014 01:28:19 GMT[\r][\n]

[\r][\n]

SUCCESS
```
