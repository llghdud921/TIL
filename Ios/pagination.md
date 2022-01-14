# pagination

### pagination이란?

TabelView나 CollectionView를 스크롤하면서 데이터를 아래로 불러오는 것입니다.

<img src= "https://user-images.githubusercontent.com/40068674/149531548-39a1f612-4d68-4142-8d17-4910d8ba7f40.gif" width="250" height="500">

<br>

### pagination 구현

여기서 필수적인 것은 UIScrollViewDelegate를 채택해야합니다.

delegate 내의 scrollViewDidScroll() 를 이용하여 구현해볼 것입니다.

```swift
func scrollViewDidScroll(_ scrollView: UIScrollView) {
    let position = scrollView.contentOffset.y
    if position > ( tableView.contentSize.height - 100 - scrollView.frame.size.height) {
        networking(pagnation: true)
    }
}
```

- scrollView.contentOffset.Y
    
    offset은 point로 x,y 좌표를 나타낼 수 있으며 스크롤이 되면 contentOffset이 변하게 됩니다.
    
    그래서 scrollToTop 하기 위해서는 해당 contentOffset을 0값으로 주는 것입니다. 
    
- tableView.contentSize.height
    
     테이블 뷰 자체의 사이즈를 나타냅니다.
    

        셀이 추가되면 증가합니다.

- scrollView.frame.size.height
    
    스크롤뷰 frame의 사이즈를 나타냅니다.
    

위의 프로퍼티를 연산하여 pagination을 조절합니다~

그래서 아래로 내리면 추가로 network하게끔 구현하였습니다.
