# reloadData

### reloadData란?

tableview에서는 row와 section을 다시 로드하는 것이고 collectionview는 collecionview의 모든 데이터를 다시 로드합니다. 

셀, 섹션 머리글 및 바닥글, 인덱스 배열 등을 포함하여 테이블을 구성하는 데 사용되는 모든 데이터를 다시 로드합니다. 다시 로드의 결과로 tableview이나 collectionview가 축소되는 경우 offset을 조정합니다.

뷰의 delegate 또는 datasource는 데이터를 완전히 다시 로드하기를 원할 때 이 메서드를 호출합니다

주의할 점은 행을 삽입하거나 삭제하는 메서드에 호출해서는 안됩니다! → `beginUpdates()` `endUpdates()`

하지만 reloadData는 테이블 뷰 전체를 완전히 처음부터 구성하기 때문에 비용이 큽니다.

그래서 tableview를 변경하는 메소드들이 있습니다.

<br>

### TableView를 변경하는 메소드

- insertRow, insertSections  
    지정된 indexPath에 추가
    
- deleteRows, deleteSections  
    지정된 indexPath에서 제거
    
- reloadRows, reloadSections  
    지정된 indexPath를 reload
    

 위 메서드들은  뷰를 변경해주는 메서드들로 실제 데이터에는 영향을 주지않습니다. delegate와 datasource에서는 이 부의 변경을 알 수 없기 때문에 오류가 발생합니다. 따라서 메소드를 호출해주기 전에 반드시 데이터 변경 먼저 해주어야 합니다.

<br>

### beginUpdates() EndUpdates()

위의 메소드를 여러번 호출하는 작업을 beginUpdates. endUpdates 를 이용해서 한번에 동시에 처리하도록 해줍니다.

```swift
tableView.beginUpdates()
// view를 변경하는 코드
tableView.endUpdates()
```

<br>

### performBatchUpdates(:completion)

beginUpdates(), endUpdates() 와 동일한 기능인 performBatchUpdates입니다.

completionHanler 클로저를 통해 여러개의 변경메서드를 그룹화하여 코드를 작성할 수 있습니다.



<br>
<br>
참고 : https://jcsoohwancho.github.io/2019-10-12-TableView%EC%9D%98-%EB%B3%80%ED%99%94%EB%A5%BC-%EC%B2%98%EB%A6%AC%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95(1)-Batch-Update/
