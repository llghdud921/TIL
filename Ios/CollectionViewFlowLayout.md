# CollectionViewFlowLayout

## CollectionViewFlowlayout이란?

UICollectionViewLayout 줄 하나로 섹션에 대한 header, footer가 있는 그리드로 항목을 구성하는 레이아웃 입니다.

 

- 레이아웃을 동적으로 조정하기 위해 CollectionViewDelegateFlowLayout을 사용할 수 있습니다.
- delegate를 사용하여 그리드항목에 대해 다양한 크기를 지정. delegate가 없으면 flowlayout에 설정한 기본값을 사용합니다.
- CollectionViewFlowlayout은 contents의 모양을 구성하기 위해 여러가지 프로퍼티를 가지고 있습니다.

### 주요 프로퍼티

- scrollDisrection  
    스크롤 방향을 정할 수 있음
    .vertical, .horizontal이 있으며  default는 .vertical입니다.
    
- minimumLineSpacing  
    줄과 줄 사이에 간격을 정할 수 있음. default는 10
    
- minimuminterItemSpacing  
    item사이에 간격을 정할 수 있음. default는 10
    
- itemsize  
    item의 size를 직접 지정할 수 있음. default는 50,50입니다.
    
- sectionInset  
    collectionview의 section의 margin을 지정할 수 있음.
    
- estimatedSize  
    셀이 예상하는 크기를 알려주는 것인데 좀 더 공부가 필요~
    

## UICollectionViewDelegateFlowLayout

CollectionViewFlowlayout와 상호작용하면서 레이아웃을 조정하는 delegate도 있습니다.  
해당 프로토콜은 UICollectionview를 채택하고 있어 추가로 채택할 필요없고   
`collectionView.delegate = self` 를 이용해 사용 가능합니다.  

### 주요 메서드

- sizeForItemAt  
    item의 사이즈를 반환하는 메소드
    
- insetForSectionAt  
    섹션의 여백을 반환하느 메소드
    
- minimumLineSpacingForSection  
    섹션의 행 사이 간격, 최소 간격을 반환하는 메소드
    
- minimumInherititemSpacingForSectionAt  
    섹션의 item 사이 최소간격을 반환하는 메소드
