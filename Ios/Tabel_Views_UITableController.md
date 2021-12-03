# Table Views, UITableController 
## Table Views란?
> Display data in a single column of customizable rows.  

단일 column내 커스텀 할 수 있는 row에 데이터를 표시한다.

<br/>

Table Views가 어디 속해있는지도 찾아보았습니다.
![스크린샷 2021-12-03 오후 6 49 45](https://user-images.githubusercontent.com/40068674/144617102-c7b7cc54-6d59-4dfe-a9f7-33b442d6e989.png)

Table Views는 Container Views 중 하나이다.

<br/>

### Container Views란?

Container Views의 의미는 찾아볼 수 없었고 
> Organize and present large data sets using container views.
> 

Container Views를 사용해서 많은 데이터 셋을 구성하고 표시할 수 있다.  
이런 설명만 있네요. 유추해보자면 Container ViewController와 비슷하지 않을까요

Container ViewController는 컨트롤러 간에 부모-자식 관계를 형성해 자식을 관리하는데요  
Container Views는 view와 부모-자식 관계를 형성하고 자식을 관리하는 View들이지 않을까 합니당... (틀릴수있음)  

<br/>

## Table Views 사용하기

table view에는 수직 스크롤 컨텐츠의 단일 column이 row과 section으로 분할되어 표시됩니다.   
테이블의 각 row에는 앱과 관련된 단일 정보가 표시되고 section을 통해 관련 row을 그룹화할 수 있습니다. 

 

- **Cells**
    cell은 내용에 대한 시각적 표현을 제공합니다. UIKit에서 제공하는 기본 cell을 사용하거나 앱의 필요에 맞게 커스텀 셀을 정의할 수 있습니다.
    
- **`Table view controller`**
    일반적으로 `UITableViewController` 개체를 사용하여 Table View를 관리합니다. 다른 ViewController도 사용할 수 있지만 일부 테이블 관련 기능이 작동하려면 `UITableViewController`가 필요합니다.
    
- **Your data source object**
    이 객체는 `UITableViewDataSource` protocol을 채택하고 테이블에 대한 데이터를 제공합니다.
    
- **Your delegate object**
    이 객체는 `UITableViewDelegate` protocol을 채택하고 table content를 가지고 User Interaction을 관리합니다.
    
<br/>

## UITableViewController는 무엇일까?

Table View를 관리하는데 특화된 ViewController이다

- 해당 ViewController에 Table View외에 다른 content가 없는 경우 주로 사용된다.
- Table view controllers는 tabel view 내용과 변경을 관리하는데 필요한 프로토콜을 이미 채택하고있다.

<br/>

### UITableViewController에서 구현할 수 있는 것

- StoryBoard 또는 nib 파일에 보관된 테이블 뷰를 자동으로 로드합니다. `table View` property를 사용하여 tableView에 액세스합니다.
- data source와 table view의 delegate를 `self`로 설정합니다.
- `viewWillAppear` 메소드를 구현하고 처음 나타날 때 table view에 대한 데이터를 자동으로 다시 로드합니다.
    
    table view가 표시될 때마다 선택 항목을 지우므로, `SelectionOnViewWillAppear` property 값을 변경하여 이 동작을 비활성화할 수 있습니다.
    
- `viewDidAppear` 메소드를 구현하고 table view의 스크롤 표시가 처음 나타날 때 자동으로 깜박입니다.
- setEditing 메소드를 구현하고 사용자가 Edit|Done 버튼을 누르면 table의 edit mode가 자동으로 전환된다.
- 화면 키보드의 모양 또는 사라짐에 따라 table view의 크기를 자동으로 조정합니다.

<br/>

### UITableViewController에서 구현시 주의할 점

- 관리하는 각 table View에 대해 UITableViewController의 custom subclass를 만듭니다.
- Table view controller를 초기화할 때 table View의 스타일(plain 또는 group)을 지정해야 합니다.
- data source를 override해야하고 데이터를 채우는 데 필요한 메서드를 delegate해야 합니다.
- loadView() 나 다른 superclass 메서드를 override할 수 있지만, override할 경우 메소드의 superclass구현을 호출해야 합니다. 일반적으로 첫번째 메소드에 호출한다.

<br/>

## UITableViewController와 UIViewController에 UITableView 만드는 것은 무슨 차이가 있을까?

둘 다 구현해본적이 없어서 잘 모르지만  
구글링해보니 아주 잘 정리된 글이 있었습니다.

**[UITableViewController vs UIViewController + UITableView](https://www.codementor.io/@nguyentruongky/uitableviewcontroller-vs-uiviewcontroller-uitableview-rfxuec34w)**

둘의 가장 큰 차이는 static cell의 지원 유무입니다.

UITableViewController은 static cell을 지원합니다.  
static cell은 말 그대로 정적인 셀. 고정되어 있는 테이블 셀을 말하는데   
UIViewController에 UITableView를 추가하는 경우 static cell을 지원하지 않습니다. 그래서 UIViewController에 UITableViewController를 만들어서 사용해야합니다. 번거롭죵

그 밖에도 현재 활성화된 textField, baseController 등등 써져있는데 직접 구현해봐야 할것같습니다.
