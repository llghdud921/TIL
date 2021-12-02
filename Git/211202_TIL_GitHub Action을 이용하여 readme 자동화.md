## GitHub Action을 이용하여 readme 자동화

<br/>

github action을 이용해서 TIL 관리를 편하게 하는 다른 repo를 보고 찾아보게되었습니다.  
우선 github action을 간단하게 알아봅시당

## GitHub Actions이란?

빌드, 테스트 및 배포 파이프라인을 자동화할 수 있는 CI/CD(지속적 통합 및 지속적 전달) 플랫폼입니다.  
자세한 내용은 [understanding-github-actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions) 를 참고하세요.  
처음 접해보는 기술이라 서투르지만 **[TIL Auto-Format README](https://github.com/marketplace/actions/til-auto-format-readme)** 를 사용해보면서 github action에 대해 한발짝 알아보겠습니다.

<br/> 

## TIL Auto-Format 사용방법

1. **내 TIL repository에 `.github/workflows/build.yml` 파일을 만듭니다.**
    ![스크린샷 2021-12-02 오후 11 55 57](https://user-images.githubusercontent.com/40068674/144454015-c75bcf44-66f2-4ae7-bf47-0450e6d867da.png)

2. **해당 파일에 [TIL Auto-Format README](https://github.com/marketplace/actions/til-auto-format-readme)에 나와있는 코드를 복사하면 됩니다.**
    ![스크린샷 2021-12-02 오후 11 55 49](https://user-images.githubusercontent.com/40068674/144455213-f1c32aee-dae2-4107-b87b-932c304f6d45.png)
    위 코드를 살펴보면
    
    - branches : 기존 코드에 master branch 로 설정되어있는데 main으로 변경 (지금은 git의 메인 브런치가 main 이기 때문에)
    - description : TIL에 대한 설명 쓸 수 있음
    
    두가지를 바꿔주었습니다.
    
    그럼 README-bot이 commit을 합니다. 시간 지연이 있어요 한 1분,,,?  
    ![스크린샷 2021-12-02 오후 11 56 35](https://user-images.githubusercontent.com/40068674/144455358-1c89520b-613f-4952-bb8e-0d1042f7440b.png)

    README 창은 아래 화면과 같이 수정됩니다.
    ![스크린샷 2021-12-02 오후 11 56 53](https://user-images.githubusercontent.com/40068674/144455565-2315e0ec-94a0-4f27-a3e1-c25d25455c2c.png)

3. **TIL 작성하기**
    
    TIL 작성할 때 주의사항은 `#` 하나를 이용해서 제목을 달아줘야합니다. 
    
    `##` 는 안대여~ 설정을 그렇게 해놓으신 것 같습니다.
    
    그리고 TIL이 폴더 안에 위치해야합니다. 폴더 안에 위치하지 않으면 README update가 안됩니다.
    ![스크린샷 2021-12-03 오전 12 31 55](https://user-images.githubusercontent.com/40068674/144455821-4f51e376-606c-4b4d-8799-a0a3b6cec681.png)
    
    이렇게 작성이 완료되면 아래 결과창이 나와요~
    
    `#` 하나만 쓰세요 두개로 test 했더니 아래처럼 잘못나오네요.  
    ![스크린샷 2021-12-03 오전 12 07 48](https://user-images.githubusercontent.com/40068674/144455884-43d2dd4f-05e9-4960-a7e0-4573ee1b0a36.png)


<br/>
그럼 자동화도 적용했으니 TIL을 열심히 써보도록 하겠습니다~


