# URL Session

관련된 네트워크 데이터 전송 작업 그룹을 조정하는 개체입니다.

<br>

### **Overview**

URLSession 클래스 및 관련 클래스는 URL로 표시된 endpoint에서 데이터를 다운로드하고 업로드하기 위한 API를 제공합니다. 또한 앱이 실행 중이 아닐 때 또는 앱이 일시 중단된 상태에서 iOS에서 백그라운드 다운로드를 수행하는 데 이 API를 사용할 수 있습니다. 관련 URLsessionDelegate 및 URLSessionTaskDelegate를 사용하여 인증을 지원하고 redirection 및 task completion와 같은 이벤트를 수신할 수 있습니다.

앱은 관련 데이터 전송 작업 그룹을 조정하는 URLSession 인스턴스를 하나 이상 만듭니다. 예를 들어 웹 브라우저를 만드는 경우 앱은 탭이나 창당 세션을 하나씩 만들거나, 대화형 세션을 만들고 백그라운드 다운로드를 위한 세션을 만들 수 있습니다. 각 세션 내에서 앱은 특정 URL에 대한 요청을 나타내는 일련의 작업을 추가합니다(필요한 경우 HTTP 리디렉션 수행).

<br>

### **Types of URL Sessions**

지정된 URLSession 내의 작업은 공통 세션 구성 개체를 공유하며, 이 구성 개체는 단일 호스트에 대한 최대 동시 연결 수, 연결이 셀룰러 네트워크를 사용할 수 있는지 여부 등과 같은 연결 동작을 정의합니다.

URLSession에는 기본 요청에 대한 싱글 톤 shared session이 있습니다. 그것은 당신이 만드는 세션만큼 사용자 정의 할 수 없지만, 당신이 매우 제한된 요구 사항을 가지고 있다면 좋은 출발점 역할을합니다. shared 클래스 메서드를 호출하여 이 세션에 액세스하라.

- default session은 shared session과 비슷하게 동작하지만 구성할 수 있습니다. 또한 default session에 delegate를 할당하여 데이터를 점진적으로 얻을 수 있습니다.
- Ephemeral sessions은 shared session과 비슷하지만 캐시, 쿠키 또는 자격 증명을 디스크에 쓰지 않습니다.
- Background sessions을 사용하면 앱이 실행되지 않는 동안 백그라운드에서 콘텐츠 업로드 및 다운로드를 수행할 수 있습니다.

<br>

### **Types of URL Session Tasks**

Session 내에서 데이터를 서버에 선택적으로 업로드한 다음 서버의 데이터를 디스크의 파일 또는 메모리에 있는 하나 이상의 NSData 개체로 검색하는 작업을 작성합니다.

- data task는 NSDATA 개체를 사용하여 데이터를 보내고 받습니다. 데이터 태스크는 서버에 대한 짧고 종종 대화형 요청을 위한 것입니다.
- upload task은 데이터 작업과 비슷하지만 데이터 (종종 파일 형식)를 보내고 앱이 실행되지 않는 동안 백그라운드 업로드를 지원합니다.
- websocket task는 RFC 6455에 정의된 웹소켓 프로토콜을 사용하여 TCP와 TLS를 통해 메시지를 교환합니다.

<br>

### **Using a Session Delegate**

세션의 작업도 delegate 개체를 공유합니다. 다음과 같은 경우를 포함하여 다양한 이벤트가 발생할 때 정보를 제공하고 얻기 위해 이 위임자를 구현합니다.

- 인증 실패
- 서버로부터 데이터가 도착했다.
- 캐싱에 데이터를 사용할 수 있다.

delegate가 제공하는 기능이 필요하지 않은 경우 세션을 만들 때 0을 입력하여 이 API를 사용할 수 있습니다.
