# TableView에서 Register 사용하기

<br/>

`tableView.dequeueReusableCell(withIdentifier: "testCell", for: indexPath)`  
셀을 재사용하기 위해서 Reuseable queue에서 꺼내기 위해서는 tableView에 register를 해야합니다.

register하는 방법으로는 storboard를 이용하거나 코드로 구현하는 방법이 있습니다.

<br/>

### StoryBoard를 이용하여 cell을 register

tableView에 tableView cell을 넣어 해당 cell에 identidfier를 지정합니다.

![스크린샷 2021-12-14 오후 10 10 35](https://user-images.githubusercontent.com/40068674/146048284-d41a1e9b-fd36-4034-9734-9993109850a1.png)

그러면 자동으로 storyboard에서 register되기 때문에 따로 register 작업을 해주지 않아도 등록됩니다.

<br/>

### storyboard에 indentifier를 지정하지 않고 cell을 register

위에 tableViewCell에 직접 indentifier를 지정하지 않고

코드로 cell의 identifier을 등록하는 방법입니다.

```swift
// direct cell register
let directIdentifier = "testCell"
    
override func viewDidLoad() {
    super.viewDidLoad()
        
    tableView.register(UITableViewCell.self, forCellReuseIdentifier: directIdentifier)
}

// dataSource 구현 코드
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: directIdentifier, for: indexPath)
```
<br/>
