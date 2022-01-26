# DataSource 코드 분리

viewController코드가 너무 방대해져서 CollectionView의 DataSource 코드를 분리해보겠습니다..

<br>

### 1. DataSource 파일 만들기

DataSource는 CollectionView에서 사용하는 클래스 타입으로 NSObject를 상속받아야합니다.

```swift
class CollectionViewDataSource: NSObject { }
```
<br>

### 2. UICollectionViewDataSource 채택하기

내부에 dataSource 관련 코드를 마구마구 구현하면 됩니다~

```swift
extension MainDataSource: UICollectionViewDataSource {
    func collectionView(
        _ collectionView: UICollectionView,
        numberOfItemsInSection section: Int
    ) -> Int {
        return productList.count
    }
    
    func collectionView(
        _ collectionView: UICollectionView,
        cellForItemAt indexPath: IndexPath
    ) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: currentCellIdentifier,for: indexPath)
        return cell
    }
}
```

<br>

### 3. dataSource가 가지고있는 data도 여기다가 저장하면 더 좋겠죠?

dataSource란 데이터를 받아 뷰를 그려주는 역할로 쓰이는데 데이터를 dataSource 내부 파일에 저장하면   
dataSource 내부 메서드 구현도 편리하고 역할도 명확해집니다.

```swift

class CollectionViewDataSource: NSObject {
    private(set) var productList: [Product] = []
}
```
