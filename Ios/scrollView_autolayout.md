# ScrollView Autolayout

<br/>

## ScrollView란?

스크롤뷰에 포함된 뷰를 스크롤 및 확대/축소할 수 있는 뷰입니다.

scrollView에는 contentLayout, frameLayout 두가지의 뷰가 같이 생성됩니다.

<br/>

### FrameLayoutGuide

스크롤 뷰의 변환되지 않은 프레임 사각형을 기반으로 한 Layout Guide입니다.

컨텐츠 직사각형이 아닌 스크롤 뷰 자체의 프레임 직사각형을 명시적으로 포함하는 AutoLayout constraints를 작성해야한다.

스크롤뷰 영역인 Frame Layout Guide를 이용해서 스크롤 뷰의 content를 고정할 수 있을때 사용한다.

- 가로로 스크롤 : View.height = frame Layout Guide.height
- 세로로 스크롤 : View.width = Frame Layout Guide.width

<br/>

### **contentLayoutGuide**

`scrollView`의 변환되지 않은 내용 사각형을 기반으로 한 Layout Guide입니다.

`scrollView`의 content 영역과 관련된 `AutoLayout` `constraints`을 작성하려면 이 Layout Guide 사용합니다.

scrollView안에 들어가는 content와 제약조건을 거는 것은 이곳에 걸라는 말인 것 같네요.

## ScrollView autoLayout 적용하는 방법

1. **ViewController에 scollView를 넣습니다.**
    
    ![스크린샷 2021-12-09 오후 11 21 16](https://user-images.githubusercontent.com/40068674/145420918-53d475e4-824b-46e7-baff-3820c3c56e5c.png)
    
    ![스크린샷 2021-12-09 오후 11 21 57](https://user-images.githubusercontent.com/40068674/145421073-6310336f-f263-4d74-ad27-a32e49b6a21d.png)
    
    여길 보면 위에서 말한 content Layout Guide, Frame Layout Guider가 같이 생성됩니다.
    
    <br/>
    
2. **safe area를 기준으로 top, leading, trailing, bottom 제약조건을 걸어줍니다.**
    
    safe area에 걸어줄 수 도 있고 superView에 걸수도 있고 bottom만 superView에 거는 경우도 있습니다.
    
    ![스크린샷 2021-12-09 오후 11 31 09](https://user-images.githubusercontent.com/40068674/145421176-af793fcc-39b3-497a-8b89-f3baed481f53.png)

    계속 constraints 경고가 나타나지만 이건 안에 View가 없어서이므로 계속 진행합니다.
    
    <br/>
    
3. **스크롤뷰에는 스크롤 할 수 있는 영역을 알려주어야합니다.** 
    
    스크롤뷰 안의 content가 스크롤 가능한 영역을 채우기 위해 content Guide Layout를 이용합니다.
    
    스크롤뷰안의 content 다루기 쉬운 방법은 scrollView내 컨테이너 뷰로 UIView를 하나 생성합니다.
    
    ![스크린샷 2021-12-09 오후 11 37 16](https://user-images.githubusercontent.com/40068674/145421252-c86bea7d-1302-46a8-a137-b560071f92b9.png)
    
    UIVIew를 하나 넣어주고 해당 VIew를 content Layout Guide에 제약조건을 걸어 스크롤 영역을 View로 영역을 잡아줍니다.
    
    ![스크린샷 2021-12-09 오후 11 39 44](https://user-images.githubusercontent.com/40068674/145421309-cfd12f19-fb8b-4430-b5cd-bda5ad9e2943.png)
    
    <br/>
    
4. 컨테이너 View인 View를 frame layout guide를 이용해서 넓이를 고정시킨다.

    ![스크린샷 2021-12-09 오후 11 48 32](https://user-images.githubusercontent.com/40068674/145421376-481cc69b-63ac-462d-8b37-809b55c60b49.png)
    
    <br/>
    
5. View의 높이를 고정시켜준다.
    
    View 내부에 어떤 View가 들어올지 모르지만 우선 height를 설정하여 View의 크기를 지정하였습니다.
    
    ![스크린샷 2021-12-09 오후 11 53 15](https://user-images.githubusercontent.com/40068674/145421436-e79bbd68-a71a-49c7-aeb8-e9818d4bfcb7.png)
    
    그러면 이제 경고가 안나타납니다 🙂
    
    내부에 View들의 크기로 인해 동적으로 변해야하므로 prioirty를 250으로 둡니다.
    
    ![스크린샷 2021-12-09 오후 11 54 47](https://user-images.githubusercontent.com/40068674/145421486-917e77b6-fcae-4b36-ab6c-34eeece09d62.png)
    
    <br/>
    
