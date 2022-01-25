# Escaping Closure 활용

아래코드와 같이 escaping closure 를 이용해서 네트워크 처리를 하고있습니다.

```swift
// 정의부
func downloadImage(with url: String, completion: @escaping (UIImage) -> Void) {

// 구현부
ImageManager.shared.downloadImage(with: newImage.url) { image in
    self.images.append(image)
}
```

이때 이미지를 여러개 받고 모든 처리후에 View를 띄워주어야했습니다.  
escaping closure는 함수 비깥에서 실행되는 메서드로  비동기로 실행됩니다.  
그럼 이 비동기 처리가 끝나고 실행할 수 있는 방법은 무엇일까요

<br>

## DispatchGroup

비동기적으로 처리되는 작업을 그룹으로 묶어, 그룹 단위로 작업상태를 추적할 수 있는 기능입니다.  
dispatch group을 사용하면 async를 그룹으로 묶어 끝나는 시점을 알 수 있습니다.  
dispatch group은 비동기에서만 사용가능합니다. 동기에서는 끝나는 시점을 예측할 수 있어서 필요없으니까요!

### group에 등록하는 방법

- async를 호출하면서 group을 지정  
    ```swift
    let group = DispatchGroup()
    DispatchQueue.main.async(group: group) 
    ```
    

- enter, leave 사용  
    async 작업을 묶을 수도 있고 completion Handler경우 이 방법을 사용할 수 밖에 없습니다.  
    ```swift
    let group = DispatchGroup()
    group.enter()
    DispatchQueue.main.async {
        group.leave()
    }
    ```
    
<br>    

### group 처리가 끝나는 시점에 수행하는 notify

dispatch group에 속한 비동기 작업의 처리가 끝나면 수행하는 notify메소드가 있습니다.  
```swift
let group = DispatchGroup()
DispatchQueue.global().async(group: group) {
}
DispatchQueue.global().async(group: group) {
}

group.notify(queue: .main) {
   // 그룹 내 모든 비동기 처리가 끝났다
}
``` 

<br>

위 두가지를 사용하여 escaping closure인 completion의 처리가 모두 끝난 후 동작을 수행하는 로직을 구성하였습니다.   

```swift
dispatchGroup.enter()
    ImageManager.shared.downloadImage(with: newImage.url) { image in
        self.images.append(image)
        dispatchGroup.leave()
    }
}
        
dispatchGroup.notify(queue: .main) {
    DispatchQueue.main.async {
        // view작업
    }
}
```
